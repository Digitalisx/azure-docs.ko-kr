---
title: 'Azure AD Connect: Azure Active Directory Connect에 대한 TLS 1.2 적용 | Microsoft Docs'
description: 이 문서에는 Azure AD Connect 및 Azure AD Sync의 모든 릴리스가 나열되어 있습니다.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ff5c75785622b43e66b808009c4674d4b2f2b50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300851"
---
# <a name="tls-12-enforcement-for-azure-ad-connect"></a>Azure AD Connect에 대한 TLS 1.2 적용

TLS(전송 계층 보안) 프로토콜 버전 1.2는 보안 통신을 제공하도록 설계된 암호화 프로토콜입니다.  TLS 프로토콜은 주로 개인 정보 보호 및 데이터 무결성을 제공하는 것을 목표로 합니다.  TLS는 [RFC 5246](https://tools.ietf.org/html/rfc5246)에 정의된 버전 1.2를 사용하여 여러 번 반복되었습니다.  Azure Active Directory Connect 버전 1.2.65.0 이상은 이제 Azure와의 통신에 TLS 1.2만 사용할 수 있도록 완벽하게 지원합니다.  이 문서에서는 Azure AD Connect 서버에서 TLS 1.2만 사용하도록 적용하는 방법에 대한 정보를 제공합니다.

## <a name="update-the-registry"></a>레지스트리 업데이트
Azure AD Connect 서버에서 TLS 1.2만 사용하도록 적용하려면 Windows 서버의 레지스트리를 업데이트해야 합니다.  Azure AD Connect 서버에서 다음 레지스트리 키를 설정합니다.

>[!IMPORTANT]
>레지스트리가 업데이트되면 변경 내용이 적용되도록 Windows 서버를 다시 시작해야 합니다.


### <a name="enable-tls-12"></a>TLS 1.2 사용
- [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\마이크로\\소프트 . 넷프레임워크\v4.0.30319]
  - "시스템 기본값TlsVersions"=dword:00000001
  - "슈유스트롱크립크립토"=워드:0000001
- [HKEY_LOCAL_MACHINE\소프트웨어\마이크로\\소프트 . 넷프레임워크\v4.0.30319]
  - "시스템 기본값TlsVersions"=dword:00000001
  - "SchUseStrongCrypto"=dword:00000001
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\서버]
  - "사용 가능"=dword:000000001
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\서버]
  - "비활성화된 기본값"=dword:000000000 
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\클라이언트]
  - "사용 가능"=dword:000000001
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\클라이언트]
  - "비활성화된 기본값"=dword:000000000

### <a name="powershell-script-to-enable-tls-12"></a>TLS 1.2를 사용하도록 설정하는 PowerShell 스크립트
다음 PowerShell 스크립트를 사용하여 Azure AD Connect 서버에서 TLS 1.2를 사용하도록 설정할 수 있습니다.

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been enabled.'
```

### <a name="disable-tls-12"></a>TLS 1.2 사용 안 함
- [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\마이크로\\소프트 . 넷프레임워크\v4.0.30319]
  - "시스템 기본값TlsVersions"=dword:000000000
  - "슈유스트롱크립크립토"=워드:0000000
- [HKEY_LOCAL_MACHINE\소프트웨어\마이크로\\소프트 . 넷프레임워크\v4.0.30319]
  - "시스템 기본값TlsVersions"=dword:000000000
  - "슈유스트롱크립크립토"=워드:00000000
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\서버]
  - "사용 가능"=dword:000000000
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\서버]
  - "비활성화된 기본값"=dword:000000001
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\클라이언트]
  - "사용 가능"=dword:000000000
- [HKEY_LOCAL_MACHINE\SYSTEM\현재 제어세트\제어\보안 제공자\SCHANNEL\프로토콜\TLS 1.2\클라이언트]
  - "비활성화된 기본값"=dword:000000001 

### <a name="powershell-script-to-disable-tls-12"></a>TLS 1.2를 사용하지 않도록 설정하는 PowerShell 스크립트
다음 PowerShell 스크립트를 사용하여 Azure AD Connect 서버에서 TLS 1.2를 사용하지 않도록 설정할 수 있습니다.

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="next-steps"></a>다음 단계
* [Azure Active Directory와 온-프레미스 ID 통합](whatis-hybrid-identity.md)
