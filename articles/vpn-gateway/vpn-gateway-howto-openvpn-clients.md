---
title: Azure VPN Gateway에 대해 OpenVPN 클라이언트를 구성하는 방법 | Microsoft Docs
description: Azure VPN Gateway에 대해 OpenVPN 클라이언트를 구성하는 단계
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: cherylmc
ms.openlocfilehash: 09ff3ccebad0baa4148e68995254c818a29d7bd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066180"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Azure VPN 게이트웨이용 OpenVPN 클라이언트 구성

이 문서에서는 **OpenVPN &reg; 프로토콜** 클라이언트를 구성하는 데 도움이 됩니다.

## <a name="before-you-begin"></a>시작하기 전에

VPN 게이트웨이에 대해 OpenVPN을 구성하는 단계를 완료했는지 확인합니다. 자세한 내용은 [Azure VPN Gateway에 대해 OpenVPN 구성](vpn-gateway-howto-openvpn.md)을 참조하세요.

[!INCLUDE [configuration steps](../../includes/vpn-gateway-vwan-config-openvpn-clients.md)]

## <a name="next-steps"></a>다음 단계

VPN 클라이언트가 다른 VNet의 리소스에 액세스할 수 있도록 하려면 VNet-VNet 문서의 지침에 따라 [vnet-vnet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) 연결을 설정합니다. 게이트웨이 및 연결에서 BGP를 사용하도록 설정해야 합니다. 그렇지 않으면 트래픽이 흐르지 않습니다.

**"오픈VPN"은 오픈VPN 의 상표입니다.**
