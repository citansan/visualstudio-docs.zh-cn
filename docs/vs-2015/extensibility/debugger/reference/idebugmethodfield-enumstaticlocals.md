---
title: IDebugMethodField::EnumStaticLocals |Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-sdk
ms.tgt_pltfrm: ''
ms.topic: article
f1_keywords:
- IDebugMethodField::EnumStaticLocals
helpviewer_keywords:
- IDebugMethodField::EnumStaticLocals method
ms.assetid: e0c522c4-f759-4c32-ae87-7abcb573e77d
caps.latest.revision: 10
ms.author: gregvanl
manager: ghogen
ms.openlocfilehash: f3cf24790550d432c328e7d271b0fbf2399021e6
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49234203"
---
# <a name="idebugmethodfieldenumstaticlocals"></a>IDebugMethodField::EnumStaticLocals
[!INCLUDE[vs2017banner](../../../includes/vs2017banner.md)]

创建方法的静态局部变量的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp#  
HRESULT EnumStaticLocals(   
   IEnumDebugFields** ppLocals  
);  
```  
  
```csharp  
int EnumStaticLocals(  
   out IEnumDebugFields ppLocals  
);  
```  
  
#### <a name="parameters"></a>参数  
 `ppLocals`  
 [out]返回[IEnumDebugFields](../../../extensibility/debugger/reference/ienumdebugfields.md)对象，表示静态局部变量的列表。 如果没有静态局部变量，则返回 null 值。  
  
## <a name="return-value"></a>返回值  
 如果成功，则返回 S_OK 或如果没有静态局部变量，则返回 S_FALSE。 否则，返回错误代码。  
  
## <a name="remarks"></a>备注  
 每个元素均[IDebugField](../../../extensibility/debugger/reference/idebugfield.md)对象，表示不同类型的静态局部变量。 调用[GetKind](../../../extensibility/debugger/reference/idebugfield-getkind.md)来确定完全什么类型的静态本地对象表示的每个对象的方法。  
  
## <a name="see-also"></a>请参阅  
 [IDebugMethodField](../../../extensibility/debugger/reference/idebugmethodfield.md)   
 [IEnumDebugFields](../../../extensibility/debugger/reference/ienumdebugfields.md)   
 [IDebugField](../../../extensibility/debugger/reference/idebugfield.md)
