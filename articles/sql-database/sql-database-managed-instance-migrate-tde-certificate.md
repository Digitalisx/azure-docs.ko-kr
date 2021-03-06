---
title: TDE 인증서 마이그레이션 - 관리되는 인스턴스
description: 투명한 데이터 암호화를 사용하여 데이터베이스의 데이터베이스 암호화 키를 보호하는 인증서를 Azure SQL Database Managed instance로 마이그레이션
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: 0f6e379287323d9353acd887cf30d5c9c0065959
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74555378"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-database-managed-instance"></a>TDE 보호 데이터베이스의 인증서를 Azure SQL Database Managed Instance로 마이그레이션

[투명한 데이터 암호화](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)로 보호된 데이터베이스를 원시 복원 옵션을 사용하여 Azure SQL Database Managed Instance로 마이그레이션하는 경우에는 데이터베이스를 복원하기 전에 온-프레미스 또는 IaaS SQL Server의 해당 인증서를 마이그레이션해야 합니다. 이 문서에서는 Azure SQL Database Managed Instance에 인증서를 수동으로 마이그레이션 과정을 단계별로 설명합니다.

> [!div class="checklist"]
> * 인증서를 개인 정보 교환(.pfx) 파일로 내보내기
> * 파일에서 Base-64 문자열로 인증서 추출
> * PowerShell cmdlet을 사용하여 업로드

완전히 관리되는 서비스를 사용하여 TDE 보호 데이터베이스와 해당 인증서를 원활하게 마이그레이션하는 대체 옵션은 [Azure Database Migration Service를 사용하여 온-프레미스 데이터베이스를 Managed Instance로 마이그레이션하는 방법](../dms/tutorial-sql-server-to-managed-instance.md)을 참조하세요.

> [!IMPORTANT]
> 마이그레이션된 인증서는 TDE 보호 데이터베이스 복원에만 사용됩니다. 복원이 완료된 직후 마이그레이션된 인증서는 인스턴스에서 설정한 투명한 데이터 암호화 유형에 따라 서비스 관리 인증서 또는 키 자격 증명 모음의 비대칭 키인 다른 보호장치로 대체됩니다.

## <a name="prerequisites"></a>사전 요구 사항

이 문서의 단계를 완료하려면 다음 필수 구성 요소가 필요합니다.

- 파일로 내보낸 인증서에 대한 액세스 권한이 있는 온-프레미스 서버나 기타 시스템에 설치된 [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) 명령줄 도구. Pvk2Pfx 도구는 [Enterprise Windows 드라이버 키트](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk)에 속하며 독립 실행형 자체 포함 명령줄 환경입니다.
- [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell) 버전 5.0 이상 설치.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

다음 항목이 있는지 확인합니다.

- Azure PowerShell [모듈이 설치 및 업데이트되었습니다.](https://docs.microsoft.com/powershell/azure/install-az-ps)
- [Az.Sql 모듈](https://www.powershellgallery.com/packages/Az.Sql).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure 리소스 관리자 모듈은 Azure SQL Database에서 계속 지원되지만 향후 모든 개발은 Az.Sql 모듈용입니다. 이러한 cmdlet에 대 한 [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)을 참조 합니다. Az 모듈 및 AzureRm 모듈의 명령에 대한 인수는 거의 동일합니다.

PowerShell에서 다음 명령을 실행하여 모듈을 설치/업데이트합니다.

```azurepowershell
Install-Module -Name Az.Sql
Update-Module -Name Az.Sql
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

설치 또는 업그레이드가 필요한 경우, [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요.

* * *

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>TDE 인증서를 개인 정보 교환(.pfx) 파일로 내보내기

인증서는 원본 SQL Server에서 직접 내보내거나 인증서가 인증서 저장소에 보관되는 경우 인증서 저장소에서 내보낼 수도 있습니다.

### <a name="export-certificate-from-the-source-sql-server"></a>원본 SQL Server에서 인증서 내보내기

다음 단계에 따라 SQL Server Management Studio를 사용하여 인증서를 내보내서 pfx 형식으로 변환합니다. 전체 단계에서 일반 이름인 *TDE_Cert*와 *full_path*가 인증서와 파일 이름 및 경로에 사용됩니다. 이 항목은 실제 이름으로 대체해야 합니다.

1. SSMS에서 새 쿼리 창을 열고 원본 SQL Server에 연결합니다.

1. 다음 스크립트를 사용하여 TDE 보호 데이터베이스를 나열하고 마이그레이션할 데이터베이스의 암호화를 보호하는 인증서의 이름을 가져옵니다.

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![TDE 인증서 목록](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

1. 다음 스크립트를 실행하여 공개 키 및 프라이빗 키 정보를 유지하면서 인증서를 파일 쌍(.cer 및 .pvk)으로 내보냅니다.

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![TDE 인증서 백업](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

1. PowerShell 콘솔에서 Pvk2Pfx 도구를 사용하여 새로 만든 파일 쌍의 인증서 정보를 개인 정보 교환(.pfx) 파일에 복사합니다.

   ```cmd
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>인증서 저장소에서 인증서 내보내기

인증서가 SQL Server 로컬 머신 인증서 저장소에 보관되는 경우 다음 단계를 사용하여 내보낼 수 있습니다.

1. PowerShell 콘솔을 열고 다음 명령을 실행하여 Microsoft Management Console의 인증서 스냅인을 엽니다.

   ```cmd
   certlm
   ```

2. 인증서 MMC 스냅인에서 개인 -> 인증서 경로를 펼쳐서 인증서 목록을 확인합니다.

3. 인증서를 마우스 오른쪽 단추로 클릭하고 내보내기...를 클릭합니다.

4. 마법사를 따라 인증서 및 프라이빗 키를 개인 정보 교환 형식으로 내보냅니다.

## <a name="upload-certificate-to-azure-sql-database-managed-instance-using-azure-powershell-cmdlet"></a>Azure PowerShell cmdlet을 사용하여 Azure SQL Database Managed Instance에 인증서 업로드

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. PowerShell에서 준비 단계부터 시작합니다.

   ```azurepowershell
   # import the module into the PowerShell session
   Import-Module Az
   # connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzSubscription
   # set subscription for the session
   Select-AzSubscription <subscriptionId>
   ```

2. 모든 준비 단계가 완료되면 다음 명령을 실행하여 Base-64로 인코딩된 인증서를 대상 Managed Instance에 업로드합니다.

   ```azurepowershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "<password>"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<resourceGroupName>" `
       -ManagedInstanceName "<managedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

*먼저 .pfx* 파일을 사용하여 [Azure 키 볼트를 설정해야](/azure/key-vault/key-vault-manage-with-cli2) 합니다.

1. PowerShell에서 준비 단계부터 시작합니다.

   ```azurecli
   # connect to Azure with an interactive dialog for sign-in
   az login

   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   az account list

   # set subscription for the session
   az account set --subscription <subscriptionId>
   ```

1. 모든 준비 단계가 완료되면 다음 명령을 실행하여 Base-64로 인코딩된 인증서를 대상 Managed Instance에 업로드합니다.

   ```azurecli
   az sql mi tde-key set --server-key-type AzureKeyVault --kid "<keyVaultId>" `
       --managed-instance "<managedInstanceName>" --resource-group "<resourceGroupName>"
   ```

* * *

이제 지정된 Managed Instance에서 인증서를 사용할 수 있고 해당 TDE 보호 데이터베이스의 백업을 성공적으로 복원할 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 문서에서는 투명한 데이터 암호화를 사용하여 데이터베이스의 인증서 보호 암호화 키를 온-프레미스 또는 IaaS SQL Server에서 Azure SQL Database Managed Instance로 마이그레이션하는 방법을 알아보았습니다.

Azure SQL Database Managed Instance로 데이터베이스 백업을 복원하는 방법을 알아보려면 [Azure SQL Database Managed Instance에 데이터베이스 백업 복원](sql-database-managed-instance-get-started-restore.md)을 참조하세요.
