---
title: 如何： 公开库提供对对象管理器的符号的列表 |Microsoft Docs
ms.custom: ''
ms.date: 11/15/2016
ms.prod: visual-studio-dev14
ms.reviewer: ''
ms.suite: ''
ms.technology:
- vs-ide-sdk
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- IVsSimpleLibrary2 interface, lists of symbols
- IVsLibrary2 interface, lists of symbols
- symbols, call browser
- lists, symbols for the object manager
- symbols, exposing lists to the object manager
ms.assetid: 19757068-bdaa-4e7e-85d6-f8ce5026a859
caps.latest.revision: 26
ms.author: gregvanl
manager: ghogen
ms.openlocfilehash: 09a13b7fecb0d1e337def074b91eb92913a65b70
ms.sourcegitcommit: 9ceaf69568d61023868ced59108ae4dd46f720ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "49263622"
---
# <a name="how-to-expose-lists-of-symbols-provided-by-the-library-to-the-object-manager"></a>如何： 公开库提供对对象管理器的符号列表
[!INCLUDE[vs2017banner](../../includes/vs2017banner.md)]

符号浏览工具中，**类视图**，**对象浏览器**，**调用浏览器**并**查找符号结果**，将为新数据传送到请求传递给[!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]对象管理器。 对象管理器查找合适的库，并请求新的符号列表。 这些库通过提供到请求的数据来响应[!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]对象管理器通过<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2>接口。 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]对象管理器调用的方法<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2>接口以获取数据，并使用它来填充或更新符号浏览工具的视图。  
  
 调用该工具，该节点已展开，或刷新该视图时，库可能会收到针对数据的请求。 第一次调用符号浏览工具时，对象管理器请求库，以提供顶级的列表。 当在用户展开列表节点时，库提供的该节点下的子级的列表。 每个对象管理器查询包含所需的项的索引。 若要显示的新列表，对象管理器必须确定列表项、 其名称、 可访问性，以及其他属性的类型中有多少项。  
  
> [!NOTE]
>  下面的托管的代码示例演示如何提供了通过实现符号的列表<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2>接口。 对象管理器调用此接口中的方法，并使用获得的数据填充或更新符号浏览工具。  
>   
>  对于本机代码符号提供程序实现，使用<xref:Microsoft.VisualStudio.Shell.Interop.IVsObjectList2>接口。  
  
## <a name="providing-lists-of-symbols-to-the-object-manager"></a>提供对对象管理器的符号的列表  
  
#### <a name="to-provide-lists-of-symbols-to-the-object-manager"></a>若要向对象管理器提供的符号列表  
  
1.  获取符号的列表中的项的数目，通过实现<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetItemCount%2A>方法。 下面的示例演示如何对象管理器获取的列表中的项目数的信息。  
  
    ```vb  
    Protected m_Methods As System.Collections.Generic.SortedList(Of String, Method) = New System.Collections.Generic.SortedList(Of String, Method)()  
  
    Public Function GetItemCount(ByRef pCount As UInteger) As Integer  
        pCount = CUInt(m_Methods.Count)  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
    ```  
  
    ```csharp  
    protected System.Collections.Generic.SortedList<string, Method> m_Methods = new System.Collections.Generic.SortedList<string, Method>();  
  
    public int GetItemCount(out uint pCount)  
    {  
        pCount = (uint)m_Methods.Count;  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    ```  
  
2.  获取有关各类别以及给定的列表项的属性的实现信息<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetCategoryField2%2A>方法。 中指定的项类别<xref:Microsoft.VisualStudio.Shell.Interop.LIB_CATEGORY>枚举。 下面的示例演示如何对象管理器获取给定类别的项的属性。  
  
    ```vb  
    Public Function GetCategoryField2(ByVal index As UInteger, ByVal Category As Integer, ByRef pfCatField As UInteger) As Integer  
        pfCatField = 0  
  
        Select Case CType(Category, LIB_CATEGORY)  
            Case LIB_CATEGORY.LC_MEMBERTYPE  
                pfCatField = CUInt(_LIBCAT_MEMBERTYPE.LCMT_METHOD)  
  
            Case LIB_CATEGORY.LC_MEMBERACCESS  
                Dim method As Method = m_Methods.Values(CInt(Fix(index)))  
  
                If method.IsPublic Then  
                    pfCatField = CUInt(_LIBCAT_MEMBERACCESS.LCMA_PUBLIC)  
                ElseIf method.IsPrivate Then  
                    pfCatField = CUInt(_LIBCAT_MEMBERACCESS.LCMA_PRIVATE)  
                ElseIf method.IsFamily Then  
                    pfCatField = CUInt(_LIBCAT_MEMBERACCESS.LCMA_PROTECTED)  
                ElseIf method.IsFamilyOrAssembly Then  
                    pfCatField = CUInt(_LIBCAT_MEMBERACCESS.LCMA_PROTECTED) Or CUInt(_LIBCAT_MEMBERACCESS.LCMA_PACKAGE)  
                Else  
                    ' Show everything else as internal.  
                    pfCatField = CUInt(_LIBCAT_MEMBERACCESS.LCMA_PACKAGE)  
                End If  
  
            Case LIB_CATEGORY.LC_VISIBILITY  
                pfCatField = CUInt(_LIBCAT_VISIBILITY.LCV_VISIBLE)  
  
            Case LIB_CATEGORY.LC_LISTTYPE  
                pfCatField = CUInt(_LIB_LISTTYPE.LLT_MEMBERS)  
  
            Case Else  
                Return Microsoft.VisualStudio.VSConstants.S_FALSE  
        End Select  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
    ```  
  
    ```csharp  
    public int GetCategoryField2(uint index, int Category, out uint pfCatField)  
    {  
        pfCatField = 0;  
  
        switch ((LIB_CATEGORY)Category)  
        {  
            case LIB_CATEGORY.LC_MEMBERTYPE:  
                pfCatField = (uint)_LIBCAT_MEMBERTYPE.LCMT_METHOD;  
                break;  
  
            case LIB_CATEGORY.LC_MEMBERACCESS:  
                {  
                    Method method = m_Methods.Values[(int)index];  
  
                    if (method.IsPublic)  
                    {  
                        pfCatField = (uint)_LIBCAT_MEMBERACCESS.LCMA_PUBLIC;  
                    }  
                    else if (method.IsPrivate)  
                    {  
                        pfCatField = (uint)_LIBCAT_MEMBERACCESS.LCMA_PRIVATE;  
                    }  
                    else if (method.IsFamily)  
                    {  
                        pfCatField = (uint)_LIBCAT_MEMBERACCESS.LCMA_PROTECTED;  
                    }  
                    else if (method.IsFamilyOrAssembly)  
                    {  
                        pfCatField = (uint)_LIBCAT_MEMBERACCESS.LCMA_PROTECTED |  
                                     (uint)_LIBCAT_MEMBERACCESS.LCMA_PACKAGE;  
                    }  
                    else  
                    {  
                        // Show everything else as internal.  
                        pfCatField = (uint)_LIBCAT_MEMBERACCESS.LCMA_PACKAGE;  
                    }  
                }  
                break;  
  
            case LIB_CATEGORY.LC_VISIBILITY:  
                pfCatField = (uint)_LIBCAT_VISIBILITY.LCV_VISIBLE;  
                break;  
  
            case LIB_CATEGORY.LC_LISTTYPE:  
                pfCatField = (uint)_LIB_LISTTYPE.LLT_MEMBERS;  
                break;  
  
            default:  
                return Microsoft.VisualStudio.VSConstants.S_FALSE;  
        }  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    ```  
  
3.  获取给定的列表项的文本表示形式通过实现<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetTextWithOwnership%2A>方法。 下面的示例演示如何获取给定项的完整名称。  
  
    ```vb  
    Public Function GetTextWithOwnership(<System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.OLE.Interop.ULONG")> ByVal index As UInteger, <System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.Shell.Interop.VSTREETEXTOPTIONS")> ByVal tto As Microsoft.VisualStudio.Shell.Interop.VSTREETEXTOPTIONS, <System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.OLE.Interop.WCHAR")> ByRef ppszText As String) As Integer  
        ppszText = m_Methods.Values(CInt(Fix(index))).FullName  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
    ```  
  
    ```csharp  
    public int GetTextWithOwnership([System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.OLE.Interop.ULONG")] uint index, [System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.Shell.Interop.VSTREETEXTOPTIONS")] Microsoft.VisualStudio.Shell.Interop.VSTREETEXTOPTIONS tto, [System.Runtime.InteropServices.ComAliasNameAttribute("Microsoft.VisualStudio.OLE.Interop.WCHAR")] out string ppszText)  
    {  
        ppszText = m_Methods.Values[(int)index].FullName;  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    ```  
  
4.  获取给定的列表项的图标信息通过实现<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetDisplayData%2A>方法。 图标表示的类型 （类、 方法和等等） 和可访问性 （私有、 公共的等等） 的列表项。 下面的示例演示如何获取基于给定的项属性的信息图标。  
  
    ```vb  
    Public Overridable Function GetDisplayData(ByVal index As UInteger, ByVal pData As Microsoft.VisualStudio.Shell.Interop.VSTREEDISPLAYDATA()) As Integer  
        If pData Is Nothing Then  
            Return Microsoft.VisualStudio.VSConstants.E_INVALIDARG  
        End If  
  
        Dim method As Method = m_Methods.Values(CInt(Fix(index)))  
  
        Dim iImage As Integer = 12 * 6 ' See env\inc\OMGlyphs.h.  
  
        Const OM_GLYPH_ACC_PUBLIC As Integer = 0  
        Const OM_GLYPH_ACC_INTERNAL As Integer = 1  
        Const OM_GLYPH_ACC_PROTECTED As Integer = 3  
        Const OM_GLYPH_ACC_PRIVATE As Integer = 4  
  
        If method.IsPublic Then  
            iImage += OM_GLYPH_ACC_PUBLIC  
        ElseIf method.IsPrivate Then  
            iImage += OM_GLYPH_ACC_PRIVATE  
        ElseIf method.IsFamily Then  
            iImage += OM_GLYPH_ACC_PROTECTED  
        ElseIf method.IsFamilyOrAssembly Then  
            iImage += OM_GLYPH_ACC_PROTECTED  
        Else  
            iImage += OM_GLYPH_ACC_INTERNAL  
        End If  
  
        pData(0).Image = CUShort(iImage)  
        pData(0).SelectedImage = CUShort(iImage)  
  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
    ```  
  
    ```csharp  
    public virtual int GetDisplayData(uint index, Microsoft.VisualStudio.Shell.Interop.VSTREEDISPLAYDATA[] pData)  
    {  
        if (pData == null)  
        {  
            return Microsoft.VisualStudio.VSConstants.E_INVALIDARG;  
        }  
  
        Method method = m_Methods.Values[(int)index];  
  
        int iImage = 12 * 6;    // See env\inc\OMGlyphs.h.  
  
        const int OM_GLYPH_ACC_PUBLIC = 0;  
        const int OM_GLYPH_ACC_INTERNAL = 1;  
        const int OM_GLYPH_ACC_PROTECTED = 3;  
        const int OM_GLYPH_ACC_PRIVATE = 4;  
  
        if (method.IsPublic)  
        {  
            iImage += OM_GLYPH_ACC_PUBLIC;  
        }  
        else if (method.IsPrivate)  
        {  
            iImage += OM_GLYPH_ACC_PRIVATE;  
        }  
        else if (method.IsFamily)  
        {  
            iImage += OM_GLYPH_ACC_PROTECTED;  
        }  
        else if (method.IsFamilyOrAssembly)  
        {  
            iImage += OM_GLYPH_ACC_PROTECTED;  
        }  
        else  
        {  
            iImage += OM_GLYPH_ACC_INTERNAL;  
        }  
  
        pData[0].Image = (ushort)iImage;  
        pData[0].SelectedImage = (ushort)iImage;  
  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    ```  
  
5.  获取有关给定的列表项是否通过实现是可展开的信息<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetExpandable3%2A>方法。 下面的示例演示如何获取给定的项是否可以通过展开的信息。  
  
    ```vb  
    Public Function GetExpandable(ByVal index As UInteger, ByRef pfExpandable As Integer) As Integer  
        pfExpandable = Microsoft.VisualStudio.VSIP.Samples.CallBrowser.Constants.TRUE  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
  
    Public Function GetExpandable3(ByVal index As UInteger, ByVal ListTypeExcluded As UInteger, ByRef pfExpandable As Integer) As Integer  
        Return GetExpandable(index, pfExpandable)  
    End Function  
    ```  
  
    ```csharp  
    public int GetExpandable(uint index, out int pfExpandable)  
    {  
        pfExpandable = Microsoft.VisualStudio.VSIP.Samples.CallBrowser.Constants.TRUE;  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    public int GetExpandable3(uint index, uint ListTypeExcluded, out int pfExpandable)  
    {  
        return GetExpandable(index, out pfExpandable);  
    }  
  
    ```  
  
6.  通过实现获取的给定的列表项的符号的子列表<xref:Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2.GetList2%2A>方法。 下面的示例演示如何获取的给定项的符号的子列表**调用**或**的调用方**关系图。  
  
    ```vb  
    ' Call graph list.  
    Public Class CallsList  
        Inherits ResultsList  
        Implements Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2  
        Public Sub New(ByVal library As Library)  
            MyBase.New(library)  
        End Sub  
  
        Public Function GetList2(ByVal index As UInteger, ByVal ListType As UInteger, ByVal flags As UInteger, ByVal pobSrch As Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2(), ByRef ppList As Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2) As Integer  
            Return MyBase.GetCallsList(index, ppList)  
        End Function  
    End Class  
  
    ' Callers graph list.  
    Public Class CallersList  
        Inherits ResultsList  
        Implements Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2  
        Public Sub New(ByVal library As Library)  
            MyBase.New(library)  
        End Sub  
  
        Public Function GetList2(ByVal index As UInteger, ByVal ListType As UInteger, ByVal flags As UInteger, ByVal pobSrch As Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2(), ByRef ppList As Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2) As Integer  
            Return MyBase.GetCallersList(index, ppList)  
        End Function  
    End Class  
  
    ' Call graph list.  
    Public Function GetCallsList(ByVal index As UInteger, ByRef ppList As Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2) As Integer  
        ppList = Nothing  
        Dim callsList As ResultsList = New CallsList(m_Library)  
  
        Dim method As Method = m_Methods.Values(CInt(Fix(index)))  
        Dim strMethod As String = method.m_strPrototype  
  
        Dim Calls As System.Collections.Generic.List(Of CallInstance) = m_Library.CallGraph.GetCallGraph(method)  
  
        For i As Integer = 0 To Calls.Count - 1  
            Dim caller As Method = Calls(i).m_Target  
            callsList.AddMethod(caller)  
        Next i  
  
        ppList = CType(callsList, Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2)  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
  
    ' Callers graph list.  
    Public Function GetCallersList(ByVal index As UInteger, ByRef ppList As Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2) As Integer  
        ppList = Nothing  
        Dim callersList As ResultsList = New CallersList(m_Library)  
  
        Dim method As Method = m_Methods.Values(CInt(Fix(index)))  
        Dim strMethod As String = method.m_strPrototype  
  
        Dim Callers As System.Collections.Generic.List(Of CallInstance) = m_Library.CallGraph.GetCallersGraph(method)  
  
        For i As Integer = 0 To Callers.Count - 1  
            Dim caller As Method = Callers(i).m_Source  
            callersList.AddMethod(caller)  
        Next i  
  
        ppList = CType(callersList, Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2)  
        Return Microsoft.VisualStudio.VSConstants.S_OK  
    End Function  
  
    ' Get a child list of symbols for a given list item.  
    Public Function GetList2(ByVal index As UInteger, ByVal ListType As UInteger, ByVal flags As UInteger, ByVal pobSrch As Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2(), ByRef ppList As Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2) As Integer  
        ppList = Nothing  
  
        Dim method As Method = m_Methods.Values(CInt(Fix(index)))  
        Dim strMethod As String = method.m_strPrototype  
  
        ' Determine if the list belongs to Call or Callers graphs.  
        If CUInt(m_nFlags And CUInt(Microsoft.VisualStudio.Shell.Interop._VSOBSEARCHOPTIONS2.VSOBSO_CALLSFROM)) > 0 Then  
            ' Build the list for the Call graph.  
            Return MyBase.GetCallsList(index, ppList)  
        ElseIf CUInt(m_nFlags And CUInt(Microsoft.VisualStudio.Shell.Interop._VSOBSEARCHOPTIONS2.VSOBSO_CALLSTO)) > 0 Then  
            ' Build the list for the Callers graph.  
            Return MyBase.GetCallersList(index, ppList)  
        End If  
  
        Return Microsoft.VisualStudio.VSConstants.E_FAIL  
    End Function  
    ```  
  
    ```csharp  
    // Call graph list.  
    public class CallsList :  
        ResultsList,  
        Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2  
    {  
        public CallsList(Library library) :  
            base(library)  
        {  
        }  
  
        public int GetList2(uint index, uint ListType, uint flags, Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2[] pobSrch, out Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2 ppList)  
        {  
            return base.GetCallsList(index, out ppList);  
        }  
    }  
  
    // Callers graph list.  
    public class CallersList :  
        ResultsList,  
        Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2  
    {  
        public CallersList(Library library) :  
            base(library)  
        {  
        }  
  
        public int GetList2(uint index, uint ListType, uint flags, Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2[] pobSrch, out Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2 ppList)  
        {  
            return base.GetCallersList(index, out ppList);  
        }  
    }  
  
    // Call graph list.  
    public int GetCallsList(uint index, out Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2 ppList)  
    {  
        ppList = null;  
        ResultsList callsList = new CallsList(m_Library);  
  
        Method method = m_Methods.Values[(int)index];  
        string strMethod = method.m_strPrototype;  
  
        System.Collections.Generic.List<CallInstance> Calls = m_Library.CallGraph.GetCallGraph(method);  
  
        for (int i = 0; i < Calls.Count; i++)  
        {  
            Method caller = Calls[i].m_Target;  
            callsList.AddMethod(caller);  
        }  
  
        ppList = (Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2)(callsList);  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    // Callers graph list.  
    public int GetCallersList(uint index, out Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2 ppList)  
    {  
        ppList = null;  
        ResultsList callersList = new CallersList(m_Library);  
  
        Method method = m_Methods.Values[(int)index];  
        string strMethod = method.m_strPrototype;  
  
        System.Collections.Generic.List<CallInstance> Callers = m_Library.CallGraph.GetCallersGraph(method);  
  
        for (int i = 0; i < Callers.Count; i++)  
        {  
            Method caller = Callers[i].m_Source;  
            callersList.AddMethod(caller);  
        }  
  
        ppList = (Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2)(callersList);  
        return Microsoft.VisualStudio.VSConstants.S_OK;  
    }  
  
    // Get a child list of symbols for a given list item.  
    public int GetList2(uint index, uint ListType, uint flags, Microsoft.VisualStudio.Shell.Interop.VSOBSEARCHCRITERIA2[] pobSrch, out Microsoft.VisualStudio.Shell.Interop.IVsSimpleObjectList2 ppList)  
    {  
        ppList = null;  
  
        Method method = m_Methods.Values[(int)index];  
        string strMethod = method.m_strPrototype;  
  
        // Determine if the list belongs to Call or Callers graphs.  
        if ((uint)(m_nFlags & (uint)Microsoft.VisualStudio.Shell.Interop._VSOBSEARCHOPTIONS2.VSOBSO_CALLSFROM) > 0)  
        {  
            // Build the list for the Call graph.  
            return base.GetCallsList(index, out ppList);  
        }  
        else if ((uint)(m_nFlags & (uint)Microsoft.VisualStudio.Shell.Interop._VSOBSEARCHOPTIONS2.VSOBSO_CALLSTO) > 0)  
        {  
            // Build the list for the Callers graph.  
            return base.GetCallersList(index, out ppList);  
        }  
  
        return Microsoft.VisualStudio.VSConstants.E_FAIL;  
    }  
  
    ```  
  
## <a name="see-also"></a>请参阅  
 [支持符号浏览工具](../../extensibility/internals/supporting-symbol-browsing-tools.md)   
 [如何： 使用对象管理器注册库](../../extensibility/internals/how-to-register-a-library-with-the-object-manager.md)   
 [如何： 确定库中的符号](../../extensibility/internals/how-to-identify-symbols-in-a-library.md)   
 [旧版语言服务扩展性](../../extensibility/internals/legacy-language-service-extensibility.md)
