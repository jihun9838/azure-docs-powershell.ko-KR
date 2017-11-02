---
title: "Azure PowerShell을 사용하여 Azure 구독 관리 | Microsoft Docs"
description: "Azure PowerShell을 사용하여 Azure 구독 관리"
keywords: "Azure PowerShell, 구독"
author: sdwheeler
ms.author: sewhee
manager: carmonm
ms.product: azure
ms.service: azure-powershell
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/30/2017
ms.openlocfilehash: 68d03ec8d1a86fb3b270d02a4697bbf9af847f2d
ms.sourcegitcommit: 226527be7cb647acfe2ea9ab151185053ab3c6db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2017
---
# <a name="manage-multiple-azure-subscriptions"></a><span data-ttu-id="32f7a-104">여러 Azure 구독 관리</span><span class="sxs-lookup"><span data-stu-id="32f7a-104">Manage multiple Azure subscriptions</span></span>

<span data-ttu-id="32f7a-105">Azure를 처음 접하는 분들은 아마도 구독을 하나만 갖고 계실 것입니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-105">If you are brand new to Azure, you probably only have a single subscription.</span></span> <span data-ttu-id="32f7a-106">하지만 한동안 Azure를 사용해 오신 분들 중에는 Azure 구독을 여러 개 만든 분도 있을 것입니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-106">But if you have been using Azure for a while, you may have created multiple Azure subscriptions.</span></span> <span data-ttu-id="32f7a-107">특정 구독에 대해 명령을 실행하도록 Azure PowerShell을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-107">You can configure Azure PowerShell to execute commands against a particular subscription.</span></span>

1. <span data-ttu-id="32f7a-108">계정의 모든 구독 목록을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-108">Get a list of all subscriptions in your account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    ```
    Environment           : AzureCloud
    Account               : username@contoso.com
    TenantId              : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionId        : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionName      : My Production Subscription
    CurrentStorageAccount :

    Environment           : AzureCloud
    Account               : username@contoso.com
    TenantId              : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionId        : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionName      : My DevTest Subscription
    CurrentStorageAccount :

    Environment           : AzureCloud
    Account               : username@contoso.com
    TenantId              : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionId        : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionName      : My Demos
    CurrentStorageAccount :
    ```

2. <span data-ttu-id="32f7a-109">기본 구독을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-109">Set the default.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "My Demos"
    ```

3. <span data-ttu-id="32f7a-110">`Get-AzureRmContext` cmdlet을 실행하여 변경 내용을 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-110">Verify the change by running the `Get-AzureRmContext` cmdlet.</span></span>

    ```powershell
    Get-AzureRmContext
    ```

    ```
    Environment           : AzureCloud
    Account               : username@contoso.com
    TenantId              : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionId        : XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    SubscriptionName      : My Demos
    CurrentStorageAccount :
    ```

<span data-ttu-id="32f7a-111">기본 구독을 설정한 이후부터는 모든 Azure PowerShell 명령이 기본 구독에 대해 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="32f7a-111">Once you set your default subscription, all subsequent Azure PowerShell commands run against this subscription.</span></span>