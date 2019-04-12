---
title: Azure PowerShell을 사용하여 Azure 구독 관리
description: Azure PowerShell을 사용하여 Azure 구독 관리
author: sptramer
ms.author: sttramer
manager: carmonm
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/04/2019
ms.openlocfilehash: 29d7c84d0ca9ae8d3e4e22f407b007d2d582f8bc
ms.sourcegitcommit: 89066b7c4b527357bb2024e1ad708df84c131804
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59364073"
---
# <a name="use-multiple-azure-subscriptions"></a><span data-ttu-id="cf8d5-103">여러 Azure 구독 사용</span><span class="sxs-lookup"><span data-stu-id="cf8d5-103">Use multiple Azure subscriptions</span></span>

<span data-ttu-id="cf8d5-104">대부분의 Azure 사용자는 단일 구독만 가집니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-104">Most Azure users will only ever have a single subscription.</span></span> <span data-ttu-id="cf8d5-105">그러나, 사용자가 여러 조직에 속해 있거나 또는 여러 그룹에 걸친 특정 리소스에 액세스하기 위해 조직이 분할된 경우 Azure 내에서 여러 구독을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-105">However, if you are part of more than one organization or your organization has divided up access to certain resources across groupings, you may have multiple subscriptions within Azure.</span></span> <span data-ttu-id="cf8d5-106">CLI는 전역적으로 그리고 명령 당 구독을 선택하는 것을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-106">The CLI supports selecting a subscription both globally and per command.</span></span>

## <a name="tenants-users-and-subscriptions"></a><span data-ttu-id="cf8d5-107">테넌트, 사용자 및 구독</span><span class="sxs-lookup"><span data-stu-id="cf8d5-107">Tenants, users, and subscriptions</span></span>

<span data-ttu-id="cf8d5-108">Azure 내에서 테넌트, 사용자 및 구독 간의 차이에 대해 약간의 혼동이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-108">You might have some confusion over the difference between tenants, users, and subscriptions within Azure.</span></span> <span data-ttu-id="cf8d5-109">_테넌트_는 전체 조직을 포괄하는 Azure Active Directory 엔터티입니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-109">A _tenant_ is the Azure Active Directory entity that encompasses a whole organization.</span></span> <span data-ttu-id="cf8d5-110">이 테넌트에는 하나 이상의 _구독_ 및 _사용자_가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-110">This tenant has at least one _subscription_ and _user_.</span></span> <span data-ttu-id="cf8d5-111">사용자는 개인이며 소속된 조직인 테넌트 하나와만 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-111">A user is an individual and is associated with only one tenant, the organization that they belong to.</span></span> <span data-ttu-id="cf8d5-112">사용자는 Azure에 로그인하여 리소스를 생성, 관리, 사용하는 계정입니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-112">Users are those accounts that sign in to Azure to create, manage, and use resources.</span></span>
<span data-ttu-id="cf8d5-113">사용자는 Azure를 포함하여 클라우드 서비스를 사용하기로 Microsoft와 계약한 여러 _구독_에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-113">A user may have access to multiple _subscriptions_, which are the agreements with Microsoft to use cloud services, including Azure.</span></span> <span data-ttu-id="cf8d5-114">모든 리소스가 구독과 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-114">Every resource is associated with a subscription.</span></span>

<span data-ttu-id="cf8d5-115">테넌트, 사용자 및 구독 간의 차이점에 대한 자세한 내용은 [Azure 클라우드 용어 사전](/azure/azure-glossary-cloud-terminology)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-115">To learn more about the differences between tenants, users, and subscriptions, see the [Azure cloud terminology dictionary](/azure/azure-glossary-cloud-terminology).</span></span>  <span data-ttu-id="cf8d5-116">새 구독을 Azure Active Directory 테넌트에 추가하는 방법을 알아보려면 [Azure 구독을 Azure Active Directory에 추가하는 방법](/azure/active-directory/active-directory-how-subscriptions-associated-directory)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-116">To learn how to add a new subscription to your Azure Active Directory tenant, see [How to add an Azure subscription to Azure Active Directory](/azure/active-directory/active-directory-how-subscriptions-associated-directory).</span></span>
<span data-ttu-id="cf8d5-117">특정 테넌트에 로그인하는 방법을 알아보려면 [Azure PowerShell로 로그인](/powershell/azure/authenticate-azureps)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-117">To learn how to sign in to a specific tenant, see [Sign in with Azure PowerShell](/powershell/azure/authenticate-azureps).</span></span>

## <a name="change-the-active-subscription"></a><span data-ttu-id="cf8d5-118">활성 구독 변경</span><span class="sxs-lookup"><span data-stu-id="cf8d5-118">Change the active subscription</span></span>

<span data-ttu-id="cf8d5-119">Azure PowerShell에서 구독 리소스에 액세스하려면, 현재 Azure 세션과 연결되어있는 구독을 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-119">In Azure PowerShell, accessing the resources for a subscription requires changing the subscription associated with your current Azure session.</span></span>
<span data-ttu-id="cf8d5-120">활성 세션 컨텍스트, 즉 어떤 테넌트, 구독 및 사용자 cmdlet이 실행되어야 하는지에 대한 정보를 수정하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-120">This is done by modifying the active session context, the information about which tenant, subscription, and user cmdlets should be run against.</span></span>
<span data-ttu-id="cf8d5-121">구독을 변경하려면 먼저 Azure PowerShell 컨텍스트 개체를 [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription)으로 검색한 다음, 현재 컨텍스트를 [Set-AzContext](/powershell/module/az.accounts/set-azcontext)로 바꾸어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-121">In order to change subscriptions, an Azure PowerShell Context object first needs to be retrieved with [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) and then the current context changed with [Set-AzContext](/powershell/module/az.accounts/set-azcontext).</span></span>

<span data-ttu-id="cf8d5-122">다음 예제는 현재 활성 상태인 테넌트에서 구독하는 방법과 이를 활성 세션으로 설정하는 방법을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-122">The next example shows how to get a subscription in the currently active tenant, and set it as the active session:</span></span>

```powershell-interactive
$context = Get-AzSubscription -SubscriptionId ...
Set-AzContext $context
```

<span data-ttu-id="cf8d5-123">컨텍스트를 저장하는 방법이나 다양한 구독으로 작업하기 쉽도록 컨텍스트를 빠르게 전환하는 방법 등 Azure PowerShell 컨텍스트에 대해 더 자세히 알아보려면, [Azure PowerShell 컨텍스트로 자격 증명 유지하기](context-persistence.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="cf8d5-123">To learn more about Azure PowerShell contexts, including how to save them and quickly switch between them for working with multiple subscriptions easily, see [Persist credentials with Azure PowerShell contexts](context-persistence.md).</span></span>