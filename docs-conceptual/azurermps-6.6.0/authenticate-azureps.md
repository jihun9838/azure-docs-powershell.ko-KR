---
title: Azure PowerShell로 로그인
description: Azure PowerShell 사용자나 서비스 주체로 로그인 또는 MSI로 로그인하는 방법.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.devlang: powershell
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: 20194ac2282d602ba61bf130791edac9f4ffae6c
ms.sourcegitcommit: fd11600079ee3844986552bccc4e274a231332b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39368146"
---
# <a name="sign-in-with-azure-powershell"></a><span data-ttu-id="9f273-103">Azure PowerShell로 로그인</span><span class="sxs-lookup"><span data-stu-id="9f273-103">Sign in with Azure PowerShell</span></span>

<span data-ttu-id="9f273-104">Azure PowerShell에서는 여러 인증 방법을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-104">Azure PowerShell supports multiple authentication methods.</span></span> <span data-ttu-id="9f273-105">가장 간단하게 시작하는 방법은 명령줄에서 대화형으로 로그인하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-105">The simplest way to get started is to sign in interactively at the command line.</span></span>

## <a name="sign-in-interactively"></a><span data-ttu-id="9f273-106">대화형으로 로그인</span><span class="sxs-lookup"><span data-stu-id="9f273-106">Sign in interactively</span></span>

<span data-ttu-id="9f273-107">대화형으로 로그인하려면 [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-107">To sign in interactively, use the [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.</span></span>

```azurepowershell
Connect-AzureRmAccount
```

<span data-ttu-id="9f273-108">실행되는 경우 이 cmdlet은 귀하의 Azure 계정과 연결된 이메일 주소와 암호를 묻는 대화 상자를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-108">When run, this cmdlet will bring up a dialog box prompting you for your email address and password associated with your Azure account.</span></span> <span data-ttu-id="9f273-109">인증될 때, 현재 PowerShell 세션에 해당 정보가 저장됩니다. 대화 상자가 닫히고 모든 Azure PowerShell cmdlet에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-109">When you authenticate, that information is saved for the current PowerShell session, the dialog is closed, and you have access to all of the Azure PowerShell cmdlets.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f273-110">Azure PowerShell 6.3.0부터는 Windows에 로그인이 유지되는 한, 여러 PowerShell 세션에서 자격 증명을 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-110">As of Azure PowerShell 6.3.0, your credentials are shared among multiple PowerShell sessions as long as you remain signed in to Windows.</span></span> <span data-ttu-id="9f273-111">자세한 내용은 [영구 자격 증명](context-persistence.md)에 대한 아티클을 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-111">For more information, see the article on [Persistent Credentials](context-persistence.md).</span></span>

## <a name="sign-in-with-a-service-principal"></a><span data-ttu-id="9f273-112">서비스 주체를 사용하여 로그인</span><span class="sxs-lookup"><span data-stu-id="9f273-112">Sign in with a service principal</span></span>

<span data-ttu-id="9f273-113">서비스 주체는 리소스를 조작하는 데 사용할 수 있는 비 대화형 계정을 만드는 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-113">Service principals provide a way for you to create non-interactive accounts that you can use to manipulate resources.</span></span> <span data-ttu-id="9f273-114">서비스 주체는 Azure Active Directory를 사용하여 규칙을 적용할 수 있는 사용자 계정과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-114">Service principals are like user accounts to which you can apply rules using Azure Active Directory.</span></span> <span data-ttu-id="9f273-115">서비스 주체에게 필요한 최소 사용 권한만 부여하여 자동화 스크립트를 훨씬 안전하게 보호할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-115">By granting the minimum permissions needed to a service principal, you can ensure your automation scripts are even more secure.</span></span>

<span data-ttu-id="9f273-116">Azure PowerShell에 사용할 서비스 주체를 생성하려면 [Azure PowerShell을 사용하여 Azure 서비스 주체 만들기](create-azure-service-principal-azureps.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="9f273-116">If you need to create a service principal for use with Azure PowerShell, see [Create an Azure service principal with Azure PowerShell](create-azure-service-principal-azureps.md).</span></span>

<span data-ttu-id="9f273-117">서비스 주체로 로그인하려면 `Connect-AzureRmAccount` cmdlet `-ServicePrincipal`인수를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-117">To sign in with a service principal, use the `-ServicePrincipal` argument with the `Connect-AzureRmAccount` cmdlet.</span></span> <span data-ttu-id="9f273-118">또한 서비스 주체의 응용 프로그램 ID, 로그인 자격 증명 및 서비스 주체와 연결된 테넌트 ID 가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-118">You will also need the service princpal's application ID, sign-in credentials, and the tenant ID associate with the service principal.</span></span> <span data-ttu-id="9f273-119">서비스 주체의 자격 증명을 적절한 개체로 가져오려면 [Get-credential](/powershell/module/microsoft.powershell.security/get-credential) cmdlet을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-119">In order to get the service principal's credentials as the appropriate object, use the [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) cmdlet.</span></span> <span data-ttu-id="9f273-120">이 cmdlet은 서비스 주체 사용자 ID와 암호를 입력하는 대화 상자를 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-120">This cmdlet will display a dialog box to enter the service principal user ID and password into.</span></span>

```azurepowershell-interactive
$pscredential = Get-Credential
Connect-AzureRmAccount -ServicePrincipal -ApplicationId  "http://my-app" -Credential $pscredential -TenantId $tenantid
```

## <a name="sign-in-using-an-azure-vm-managed-service-identity"></a><span data-ttu-id="9f273-121">Azure VM 관리 서비스 ID를 사용하여 로그인</span><span class="sxs-lookup"><span data-stu-id="9f273-121">Sign in using an Azure VM Managed Service Identity</span></span>

<span data-ttu-id="9f273-122">MSI(관리 서비스 ID)는 Azure Active Directory의 미리 보기 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-122">Managed Service Identity (MSI) is a preview feature of Azure Active Directory.</span></span> <span data-ttu-id="9f273-123">로그인에 MSI 서비스 주체를 사용할 수 있으며, 다른 리소스에 액세스하려면 앱 전용 액세스 토큰이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-123">You can use an MSI service principal for sign-in, and acquire an app-only access token to access other resources.</span></span> <span data-ttu-id="9f273-124">MSI는 Azure 클라우드에서 실행 중인 가상 머신에서만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-124">MSI is only available on virtual machines running in an Azure cloud.</span></span>

<span data-ttu-id="9f273-125">MSI에 대한 자세한 내용은 [로그인 및 토큰 획득에 대한 Azure VM MSI(관리 서비스 ID)를 사용하는 방법](/azure/active-directory/msi-how-to-get-access-token-using-msi)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="9f273-125">For more information about MSI, see [How to use an Azure VM Managed Service Identity (MSI) for sign-in and token acquisition](/azure/active-directory/msi-how-to-get-access-token-using-msi).</span></span>

## <a name="sign-in-to-another-cloud"></a><span data-ttu-id="9f273-126">다른 클라우드로 로그인</span><span class="sxs-lookup"><span data-stu-id="9f273-126">Sign in to another Cloud</span></span>

<span data-ttu-id="9f273-127">Azure Cloud Services는 다양한 지역의 데이터 처리 규정에 부합하는 다양한 환경을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-127">Azure cloud services provide different environments that adhere to the data-handling regulations of various regions.</span></span> <span data-ttu-id="9f273-128">Azure 계정이 이러한 지역과 관련된 클라우드에 있는 경우, 로그인 시 해당 환경을 지정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-128">If your Azure account is in a cloud associated with one of these regions, you need to specify the environment when you sign in.</span></span> <span data-ttu-id="9f273-129">예를 들어 계정이 중국 클라우드에 있으면 다음 명령을 사용하여 로그온합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-129">For example, if you account is in the China cloud you sign on using the following command:</span></span>

```azurepowershell-interactive
Connect-AzureRmAccount -Environment AzureChinaCloud
```

<span data-ttu-id="9f273-130">사용 가능한 환경 목록을 가져오려면 다음 명령을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="9f273-130">Use the following command to get a list of available environments:</span></span>

```azurepowershell-interactive
Get-AzureRmEnvironment | Select-Object Name
```

## <a name="learn-more-about-managing-azure-role-based-access"></a><span data-ttu-id="9f273-131">Azure 역할 기반 액세스를 관리하는 방법에 대해 알아보기</span><span class="sxs-lookup"><span data-stu-id="9f273-131">Learn more about managing Azure role-based access</span></span>

<span data-ttu-id="9f273-132">Azure에서 인증 및 구독 관리에 대한 자세한 내용은 [계정, 구독 및 관리 역할 관리](/azure/active-directory/role-based-access-control-configure)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="9f273-132">For more information about authentication and subscription management in Azure, see [Manage Accounts, Subscriptions, and Administrative Roles](/azure/active-directory/role-based-access-control-configure).</span></span>

<span data-ttu-id="9f273-133">역할 관리를 위한 Azure PowerShell cmdlet:</span><span class="sxs-lookup"><span data-stu-id="9f273-133">Azure PowerShell cmdlets for role management:</span></span>

* [<span data-ttu-id="9f273-134">Get-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="9f273-134">Get-AzureRmRoleAssignment</span></span>](/powershell/module/AzureRM.Resources/Get-AzureRmRoleAssignment)
* [<span data-ttu-id="9f273-135">Get-AzureRmRoleDefinition</span><span class="sxs-lookup"><span data-stu-id="9f273-135">Get-AzureRmRoleDefinition</span></span>](/powershell/module/AzureRM.Resources/Get-AzureRmRoleDefinition)
* [<span data-ttu-id="9f273-136">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="9f273-136">New-AzureRmRoleAssignment</span></span>](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment)
* [<span data-ttu-id="9f273-137">New-AzureRmRoleDefinition</span><span class="sxs-lookup"><span data-stu-id="9f273-137">New-AzureRmRoleDefinition</span></span>](/powershell/module/AzureRM.Resources/New-AzureRmRoleDefinition)
* [<span data-ttu-id="9f273-138">Remove-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="9f273-138">Remove-AzureRmRoleAssignment</span></span>](/powershell/module/AzureRM.Resources/Remove-AzureRmRoleAssignment)
* [<span data-ttu-id="9f273-139">Remove-AzureRmRoleDefinition</span><span class="sxs-lookup"><span data-stu-id="9f273-139">Remove-AzureRmRoleDefinition</span></span>](/powershell/module/AzureRM.Resources/Remove-AzureRmRoleDefinition)
* [<span data-ttu-id="9f273-140">Set-AzureRmRoleDefinition</span><span class="sxs-lookup"><span data-stu-id="9f273-140">Set-AzureRmRoleDefinition</span></span>](/powershell/moduel/AzureRM.Resources/Set-AzureRmRoleDefinition)