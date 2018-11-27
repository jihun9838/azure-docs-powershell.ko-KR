---
title: Azure PowerShell cmdlet 쿼리 결과
description: Azure에서 리소스에 대한 쿼리 및 결과 형식을 지정하는 방법입니다.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.devlang: powershell
ms.topic: conceptual
ms.date: 09/11/2018
ms.openlocfilehash: 6bd1bea43303e9f5a2b46d63a3ac51b4c4031b9f
ms.sourcegitcommit: 80a3da199954d0ab78765715fb49793e89a30f12
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52259605"
---
# <a name="query-output-of-azure-powershell-cmdlets"></a><span data-ttu-id="b2051-103">Azure PowerShell cmdlet 쿼리 결과</span><span class="sxs-lookup"><span data-stu-id="b2051-103">Query output of Azure PowerShell cmdlets</span></span>

<span data-ttu-id="b2051-104">기본 제공 cmdlet을 사용하여 PowerShell에서 쿼리를 완료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-104">Querying in PowerShell can be completed by using built-in cmdlets.</span></span> <span data-ttu-id="b2051-105">PowerShell에서 cmdlet은 **_동사-명사_** 형태로 이름을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-105">In PowerShell, cmdlet names take the form of **_Verb-Noun_**.</span></span> <span data-ttu-id="b2051-106">**_Get_** 동사를 사용하는 cmdlet은 cmdlet 쿼리입니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-106">The cmdlets using the verb **_Get_** are the query cmdlets.</span></span> <span data-ttu-id="b2051-107">cmdlet 명사는 cmdlet 동사가 역할을 담당하는 Azure 리소스의 종류입니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-107">The cmdlet nouns are the types of Azure resources that are acted upon by the cmdlet verbs.</span></span>

## <a name="select-simple-properties"></a><span data-ttu-id="b2051-108">단순 속성 선택</span><span class="sxs-lookup"><span data-stu-id="b2051-108">Select simple properties</span></span>

<span data-ttu-id="b2051-109">Azure PowerShell에는 각 cmdlet에 대해 정의된 기본 형식이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-109">Azure PowerShell has default formatting defined for each cmdlet.</span></span> <span data-ttu-id="b2051-110">각 리소스 형식의 가장 일반적인 속성은 자동으로 테이블 또는 목록 형식으로 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-110">The most common properties for each resource type are displayed in a table or list format automatically.</span></span> <span data-ttu-id="b2051-111">출력의 서식을 지정하는 방법에 대한 자세한 내용은 [쿼리 결과 서식 지정](formatting-output.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="b2051-111">For more information about formatting output, see [Formatting query results](formatting-output.md).</span></span>

<span data-ttu-id="b2051-112">`Get-AzureRmVM` cmdlet을 사용하여 계정에서 VM 목록을 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-112">Use the `Get-AzureRmVM` cmdlet to query for a list of VMs in your account.</span></span>

```azurepowershell-interactive
Get-AzureRmVM
```

<span data-ttu-id="b2051-113">기본 출력의 서식은 자동으로 테이블로 지정됩니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-113">The default output is automatically formatted as a table.</span></span>

```output
ResourceGroupName          Name   Location          VmSize  OsType              NIC ProvisioningState
-----------------          ----   --------          ------  ------              --- -----------------
MYWESTEURG        MyUnbuntu1610 westeurope Standard_DS1_v2   Linux myunbuntu1610980         Succeeded
MYWESTEURG          MyWin2016VM westeurope Standard_DS1_v2 Windows   mywin2016vm880         Succeeded
```

<span data-ttu-id="b2051-114">`Select-Object` cmdlet을 사용하여 흥미로운 특정 속성을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-114">The `Select-Object` cmdlet can be used to select the specific properties that are interesting to you.</span></span>

```azurepowershell-interactive
Get-AzureRmVM | Select Name,ResourceGroupName,Location
```

```output
Name          ResourceGroupName Location
----          ----------------- --------
MyUnbuntu1610 MYWESTEURG        westeurope
MyWin2016VM   MYWESTEURG        westeurope
```

## <a name="select-complex-nested-properties"></a><span data-ttu-id="b2051-115">복잡한 중첩 속성 선택</span><span class="sxs-lookup"><span data-stu-id="b2051-115">Select complex nested properties</span></span>

<span data-ttu-id="b2051-116">원하는 속성이 JSON 출력에 중첩된 경우 해당 속성의 전체 경로를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-116">If the property you want is nested in the JSON output, you need to supply the full path to the property.</span></span> <span data-ttu-id="b2051-117">다음 예제에서는 `Get-AzureRmVM` cmdlet에서 VM 이름 및 OS 형식을 선택하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-117">The following example shows how to select the VM Name and the OS type from the `Get-AzureRmVM` cmdlet.</span></span>

```azurepowershell-interactive
Get-AzureRmVM | Select Name,@{Name='OSType'; Expression={$_.StorageProfile.OSDisk.OSType}}
```

```output
Name           OSType
----           ------
MyUnbuntu1610   Linux
MyWin2016VM   Windows
```

## <a name="filter-results-with-the-where-object-cmdlet"></a><span data-ttu-id="b2051-118">Where-Object cmdlet으로 결과 필터링</span><span class="sxs-lookup"><span data-stu-id="b2051-118">Filter results with the Where-Object cmdlet</span></span>

<span data-ttu-id="b2051-119">`Where-Object` cmdlet을 사용하면 속성 값에 기반하여 결과를 필터링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-119">The `Where-Object` cmdlet allows you to filter the result based on any property value.</span></span> <span data-ttu-id="b2051-120">다음 예제에서 필터는 해당 이름에 "RGD"라는 텍스트가 포함된 VM만 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-120">In the following example, the filter selects only VMs that have the text "RGD" in their name.</span></span>

```azurepowershell-interactive
Get-AzureRmVM | Where ResourceGroupName -like RGD* | Select ResourceGroupName,Name
```

```output
ResourceGroupName  Name
-----------------  ----
RGDEMO001          KBDemo001VM
RGDEMO001          KBDemo020
```

<span data-ttu-id="b2051-121">다음 예제의 결과는 vmSize 'Standard_DS1_V2'가 포함된 VM을 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="b2051-121">With the next example, the results will return the VMs that have the vmSize 'Standard_DS1_V2'.</span></span>

```azurepowershell-interactive
Get-AzureRmVM | Where vmSize -eq Standard_DS1_V2
```

```output
ResourceGroupName          Name     Location          VmSize  OsType              NIC ProvisioningState
-----------------          ----     --------          ------  ------              --- -----------------
MYWESTEURG        MyUnbuntu1610   westeurope Standard_DS1_v2   Linux myunbuntu1610980         Succeeded
MYWESTEURG          MyWin2016VM   westeurope Standard_DS1_v2 Windows   mywin2016vm880         Succeeded
```