# eSSearch



Alias Code Creating

  Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim getData = client.GetAlias(Function(x) x.Alias("esdata"))

        If getData.Indices.Count > 0 Then
            If getData.Indices.Keys(0) = "netadata" Then
                indexName = "netadatanew"
            Else
                indexName = "netadata"
            End If
            client.CreateIndex(indexName.ToString(), Function(x) x.Mappings(Function(y) y.Map(Of NetaMapping)(Function(k) k.AutoMap())))
        Else
            indexName = "netadata"
             client.CreateIndex(indexName.ToString(), Function(x) x.Aliases(Function(j) j.Alias("esdata")).Mappings(Function(y) y.Map(Of NetaMapping)(Function(k) k.AutoMap())))
        End If

 If indexName = "netadata" Then


            client.Alias(Function(x) x.Remove(Function(y) y.Index("netadatanew").Alias("esdata")))
            client.Alias(Function(x) x.Add(Function(y) y.Index("netadata").Alias("esdata")))
            client.DeleteIndex("netadatanew")
        Else

            client.Alias(Function(x) x.Remove(Function(y) y.Index("netadata").Alias("esdata")))
            client.Alias(Function(x) x.Add(Function(y) y.Index("netadatanew").Alias("esdata")))
            client.DeleteIndex("netadata")
        End If
        
        
        Imports Nest
Imports System.Data.DataTableExtensions
Imports System.ComponentModel
Imports DataLibrary
Imports System.Data.Entity


Public Class Items
    Inherits DatabaseClasses.DatabaseObject

    Protected ItemID As Integer
    Public ThumbnailPath As String
    Public ImagePath As String
    Public ItemName As String
    Public RetailPrice As Decimal
    Public ShortDesc As String
    Public LongDesc As String
    Public VendorID As Integer
    Public Active As Boolean
    Public Featured As Boolean
    Public Archive As Boolean
    Public ItemNumber As String
    Public CategoryID As Integer
    Public SubCategoryID As Integer
    Public PageTitle As String
    Public MetaTag As String
    Public Keywords As String
    Public PageTitleDefault As Boolean
    Public MetaTagDefault As Boolean
    Public KeywordsDefault As Boolean
    Public Option1 As Boolean
    Public Option2 As Boolean
    Public SpecialPrice As String
    Public SpecialID As Integer
    Public SellingPrice As String
    Public Weight As Double
    Public ManufacturerPartNumber As String
    Public ProductLineID As Integer
    Public CategoryIDLevel3 As Integer
    Public CategoryIDLevel4 As Integer
    Public ThumbnailURL As String
    Public LargeImageURL As String
    'Used for item import not apart of the structure of the table
    Public Vendor As String
    Public Category As String
    Public SubCategory As String
    Public CategoryLevel3 As String
    Public CategoryLevel4 As String
    Public Special As String
    Public ProductLine As String
    Public UNSPSCCode As String



    Sub New()
        ItemID = 0
        ThumbnailPath = ""
        ImagePath = ""
        ItemName = ""
        ShortDesc = ""
        ItemNumber = ""
        LongDesc = ""
        VendorID = 0
        Active = True
        Active = False
        Archive = False
        CategoryID = 0
        SubCategoryID = 0
        Vendor = ""
        Category = ""
        SubCategory = ""
        PageTitle = ""
        MetaTag = ""
        Keywords = ""
        PageTitleDefault = True
        MetaTagDefault = True
        KeywordsDefault = True
        Option1 = False
        Option2 = False
        SpecialID = 0
        ManufacturerPartNumber = ""
        ProductLineID = 0
        CategoryIDLevel3 = 0
        CategoryIDLevel4 = 0
        ThumbnailURL = ""
        LargeImageURL = ""
        UNSPSCCode = ""
    End Sub
    Public Sub ArchiveItem()
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        DatabaseClasses.DatabaseConnect.QueryDb("Items_Archive", paramrry)

    End Sub
    Public Sub New(ByVal ItemID As Integer)
        If ItemID > 0 Then
            Me.ItemID = ItemID
            GetByID()
        End If
    End Sub
    Public Function InsertFromImport() As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailPath", ThumbnailPath))
        paramrry.Add(New SqlClient.SqlParameter("@ImagePath", ImagePath))
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@ShortDesc", ShortDesc))
        paramrry.Add(New SqlClient.SqlParameter("@LongDesc", LongDesc))
        paramrry.Add(New SqlClient.SqlParameter("@Vendor", Vendor))
        paramrry.Add(New SqlClient.SqlParameter("@Active", Active))
        'paramrry.Add(New SqlClient.SqlParameter("@Archive", Archive))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@Category", Category))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategory", SubCategory))
        paramrry.Add(New SqlClient.SqlParameter("@Featured", Featured))
        paramrry.Add(New SqlClient.SqlParameter("@Special", Special))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryLevel3", CategoryLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryLevel4", CategoryLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@Keywords", Keywords))
        paramrry.Add(New SqlClient.SqlParameter("@ProductLine", ProductLine))
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailURL", ThumbnailURL))
        paramrry.Add(New SqlClient.SqlParameter("@LargeImageURL", LargeImageURL))
        paramrry.Add(New SqlClient.SqlParameter("@UNSPSCCode", UNSPSCCode))

        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next
        'ItemID = DatabaseClasses.DatabaseConnect.QueryDb("Items_InsertfromImport", paramrry)
        ItemID = DatabaseClasses.DatabaseConnect.QueryDb("tmpItems_InsertfromImport", paramrry)
        Return ItemID
    End Function

    Public Function Insert() As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailPath", ThumbnailPath))
        paramrry.Add(New SqlClient.SqlParameter("@ImagePath", ImagePath))
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@ShortDesc", ShortDesc))
        paramrry.Add(New SqlClient.SqlParameter("@LongDesc", LongDesc))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@Active", Active))
        paramrry.Add(New SqlClient.SqlParameter("@Featured", Featured))
        paramrry.Add(New SqlClient.SqlParameter("@Archive", Archive))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@PageTitle", PageTitle))
        paramrry.Add(New SqlClient.SqlParameter("@MetaTag", MetaTag))
        paramrry.Add(New SqlClient.SqlParameter("@Keywords", Keywords))
        paramrry.Add(New SqlClient.SqlParameter("@PageTitleDefault", PageTitleDefault))
        paramrry.Add(New SqlClient.SqlParameter("@MetaTagDefault", MetaTagDefault))
        paramrry.Add(New SqlClient.SqlParameter("@KeywordsDefault", KeywordsDefault))
        paramrry.Add(New SqlClient.SqlParameter("@Option1", Option1))
        paramrry.Add(New SqlClient.SqlParameter("@Option2", Option2))
        paramrry.Add(New SqlClient.SqlParameter("@SpecialID", SpecialID))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ProductLineID", ProductLineID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailURL", ThumbnailURL))
        paramrry.Add(New SqlClient.SqlParameter("@LargeImageURL", LargeImageURL))
        paramrry.Add(New SqlClient.SqlParameter("@UNSPSCCode", UNSPSCCode))
        ItemID = DatabaseClasses.DatabaseConnect.QueryDb("Items_Insert", paramrry)
        Return ItemID
    End Function

    Public Sub Update()
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailPath", ThumbnailPath))
        paramrry.Add(New SqlClient.SqlParameter("@ImagePath", ImagePath))
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@ShortDesc", ShortDesc))
        paramrry.Add(New SqlClient.SqlParameter("@LongDesc", LongDesc))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@Active", Active))
        paramrry.Add(New SqlClient.SqlParameter("@Featured", Featured))
        paramrry.Add(New SqlClient.SqlParameter("@Archive", Archive))
        paramrry.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@PageTitle", PageTitle))
        paramrry.Add(New SqlClient.SqlParameter("@MetaTag", MetaTag))
        paramrry.Add(New SqlClient.SqlParameter("@Keywords", Keywords))
        paramrry.Add(New SqlClient.SqlParameter("@PageTitleDefault", PageTitleDefault))
        paramrry.Add(New SqlClient.SqlParameter("@MetaTagDefault", MetaTagDefault))
        paramrry.Add(New SqlClient.SqlParameter("@KeywordsDefault", KeywordsDefault))
        paramrry.Add(New SqlClient.SqlParameter("@Option1", Option1))
        paramrry.Add(New SqlClient.SqlParameter("@Option2", Option2))
        paramrry.Add(New SqlClient.SqlParameter("@SpecialID", SpecialID))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ProductLineID", ProductLineID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@ThumbnailURL", ThumbnailURL))
        paramrry.Add(New SqlClient.SqlParameter("@LargeImageURL", LargeImageURL))
        paramrry.Add(New SqlClient.SqlParameter("@UNSPSCCode", UNSPSCCode))
        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next

        DatabaseClasses.DatabaseConnect.QueryDb("Items_Update", paramrry)
    End Sub

    Public Sub ToggleActive()
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        DatabaseClasses.DatabaseConnect.QueryDb("Items_ToggleActive", paramrry)
    End Sub

    Public Shared Function SearchItems(Optional ByVal ItemName As String = "", Optional ByVal Active As String = "", Optional ByVal ItemNumber As String = "", Optional ByVal CategoryID As Integer = 0, Optional ByVal SubCategoryID As Integer = 0, Optional ByVal VendorID As Integer = 0, Optional ByVal OrderBy As String = "", Optional ByVal PageIndex As Integer = -1, Optional ByVal NumRows As Integer = 0, Optional ByVal Featured As Integer = -1, Optional ByVal PriceRangeID As Integer = 0, Optional ByVal CategoryIDLevel3 As Integer = 0, Optional ByVal CategoryIDLevel4 As Integer = 0, Optional ByVal SpecialID As Integer = 0) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@Featured", Featured))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@OrderBy", OrderBy))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        paramrry.Add(New SqlClient.SqlParameter("@SpecialID", SpecialID))
        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next
        'System.Web.HttpContext.Current.Response.End()
        Dim dt As New DataTable

        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        Dim results
        Dim sortField = "itemID"
        Dim sOrder = 0

        If Not OrderBy = "" Then

            sortField = OrderBy.Split(" ")(0).Substring(0, 1).ToLower() & OrderBy.Split(" ")(0).Substring(1)
            If OrderBy.Split(" ")(1) = "ASC" Then
                sOrder = 0
            Else
                sOrder = 1
            End If


        End If

        If SubCategoryID = 0 Then
            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("vendorID", VendorID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            End If
        ElseIf CategoryIDLevel3 = 0 Then

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("vendorID", VendorID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            End If

        ElseIf CategoryIDLevel4 = 0 Then

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("vendorID", VendorID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            End If


        Else

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("vendorID", VendorID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
            End If


        End If


        For Each obj As NetaMapping In results.Documents
            obj.TotalCount = results.Total
            obj.SpecialPrice = obj.SpecialPriceNested.Where(Function(x) x.GroupID = CID).Select(Function(x) x.SpecialPrice).FirstOrDefault()
        Next
        dt = ConvertToDataTable(Of NetaLibrary.NetaMapping)(results.Documents)
        Return dt
    End Function
    Public Shared Function FilterDataTableRow(ByVal dt As DataTable, ByVal startRowIndex As Integer, ByVal endRowIndex As Integer) As DataTable
        Dim dataRow As DataRow() = dt.Select("Row >=" & startRowIndex & "and Row <=" & endRowIndex)
        Dim filterTable As New DataTable
        filterTable = dt.Clone()
        For Each dr As DataRow In dataRow
            filterTable.Rows.Add(dr.ItemArray)
        Next
        Return filterTable
    End Function

    Public Shared Function SearchItemsCategory(Optional ByVal Active As String = "", Optional ByVal CategoryID As Integer = 0, Optional ByVal SubCategoryID As Integer = 0, Optional ByVal OrderBy As String = "", Optional ByVal PageIndex As Integer = -1, Optional ByVal NumRows As Integer = 0, Optional ByVal CategoryIDLevel3 As Integer = 0, Optional ByVal CategoryIDLevel4 As Integer = 0) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@OrderBy", OrderBy))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next
        'System.Web.HttpContext.Current.Response.End()

        Dim dt As New DataTable

        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        Dim results
        Dim sortField = "itemID"
        Dim sOrder = 0

        If Not OrderBy = "" Then

            sortField = OrderBy.Split(" ")(0).Substring(0, 1).ToLower() & OrderBy.Split(" ")(0).Substring(1)
            If OrderBy.Split(" ")(1) = "ASC" Then
                sOrder = 0
            Else
                sOrder = 1
            End If


        End If

        If SubCategoryID = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf CategoryIDLevel3 = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf CategoryIDLevel4 = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        Else
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        End If


        For Each obj As NetaMapping In results.Documents
            obj.TotalCount = results.Total
            obj.SpecialPrice = obj.SpecialPriceNested.Where(Function(x) x.GroupID = CID).Select(Function(x) x.SpecialPrice).FirstOrDefault()
        Next
        dt = ConvertToDataTable(Of NetaLibrary.NetaMapping)(results.Documents)

        Return dt
    End Function

    Public Shared Function GetSearchCount(Optional ByVal ItemName As String = "", Optional ByVal Active As String = "", Optional ByVal ItemNumber As String = "", Optional ByVal CategoryID As Integer = 0, Optional ByVal SubCategoryID As Integer = 0, Optional ByVal VendorID As Integer = 0, Optional ByVal Featured As Integer = -1, Optional ByVal PriceRangeID As Integer = 0, Optional ByVal CategoryIDLevel3 As Integer = 0, Optional ByVal CategoryIDLevel4 As Integer = 0) As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@Featured", Featured))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim dt As New DataTable

        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        Dim results
        Dim sortField = "itemID"
        Dim sOrder = 0


        If SubCategoryID = 0 Then
            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("vendorID", VendorID)))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)))
            End If
        ElseIf CategoryIDLevel3 = 0 Then

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("vendorID", VendorID)))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)))
            End If

        ElseIf CategoryIDLevel4 = 0 Then

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("vendorID", VendorID)))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3)))
            End If


        Else

            If Not PriceRangeID = 0 And Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not PriceRangeID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("priceRangeID", PriceRangeID)))
            ElseIf Not VendorID = 0 Then
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4) And d.Term("vendorID", VendorID)))
            Else
                results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4)))
            End If


        End If


        Return results.Total

    End Function

    Public Shared Function GetSearchCountCategory(Optional ByVal Active As String = "", Optional ByVal CategoryID As Integer = 0, Optional ByVal SubCategoryID As Integer = 0, Optional ByVal CategoryIDLevel3 As Integer = 0, Optional ByVal CategoryIDLevel4 As Integer = 0) As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel3", CategoryIDLevel3))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryIDLevel4", CategoryIDLevel4))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next
        'System.Web.HttpContext.Current.Response.End()
        Dim dt As New DataTable

        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        Dim results
        Dim sortField = "itemID"
        Dim sOrder = 0



        If SubCategoryID = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID)).Sort(Function(u) u.Field("_score", SortOrder.Ascending)))
        ElseIf CategoryIDLevel3 = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID)))
        ElseIf CategoryIDLevel4 = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3)))
        Else
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Term("categoryID", CategoryID) And d.Term("subCategoryID", SubCategoryID) And d.Term("categoryIDLevel3", CategoryIDLevel3) And d.Term("categoryIDLevel4", CategoryIDLevel4)))
        End If

        Return results.Total
    End Function


    Public Shared Function GetItems(ByVal ItemName As String, ByVal Active As String, ByVal ItemNumber As String, ByVal CategoryName As String) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        'paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        'paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryName", CategoryName))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_Get", paramrry)
        Return dt
    End Function

    Public Shared Function GetItemsWithPaging(ByVal ItemName As String, ByVal Active As String, ByVal ItemNumber As String, ByVal CategoryName As String, Optional ByVal PageIndex As Integer = -1, Optional ByVal NumRows As Integer = 0, Optional ByVal ManufacturerPartNumber As String = "") As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        'paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        'paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryName", CategoryName))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        Return DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetWithPaging", paramrry)
    End Function

    Public Shared Function SimpleSearch(ByVal SearchString As String, Optional ByVal PageIndex As Integer = 0, Optional ByVal NumRows As Integer = 0, Optional ByVal Sort As String = "", Optional ByVal PriceRangeID As Integer = 0, Optional ByVal VendorID As Integer = 0) As DataTable
        Dim dt As New DataTable

        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@SearchString", SearchString))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))
        paramrry.Add(New SqlClient.SqlParameter("@Sort", Sort))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))

        'CreateIndex
        ' CreateIndex()

        ' LoadData(paramrry)

        'InsertData()
        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)
        Dim results

        If Not PriceRangeID = 0 And Not VendorID = 0 Then

            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.QueryString(Function(q) q.Query(SearchString)) And d.Term("vendorID", VendorID) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).From(PageIndex * 10).Size(NumRows))
        ElseIf Not PriceRangeID = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.QueryString(Function(q) q.Query(SearchString)) And d.Term("priceRangeID", PriceRangeID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).From(PageIndex * 10).Size(NumRows))
        ElseIf Not VendorID = 0 Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.QueryString(Function(q) q.Query(SearchString)) And d.Term("vendorID", VendorID)).Sort(Function(u) u.Field("_score", SortOrder.Descending)).From(PageIndex * 10).Size(NumRows))

        Else
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.QueryString(Function(q) q.Query(SearchString))).Sort(Function(u) u.Field("_score", SortOrder.Descending)).From(PageIndex * 10).Size(NumRows))
        End If

        For Each obj As NetaMapping In results.Documents
            obj.TotalCount = results.Total
            obj.SpecialPrice = obj.SpecialPriceNested.Where(Function(x) x.GroupID = CID).Select(Function(x) x.SpecialPrice).FirstOrDefault()
        Next
        dt = ConvertToDataTable(Of NetaLibrary.NetaMapping)(results.Documents)


        Return dt
    End Function



    
    Public Shared Function ConvertToDataTable(Of T)(data As IList(Of T)) As DataTable
        Dim properties As PropertyDescriptorCollection = TypeDescriptor.GetProperties(GetType(T))
        Dim table As New DataTable()
        For Each prop As PropertyDescriptor In properties
            table.Columns.Add(prop.Name, If(Nullable.GetUnderlyingType(prop.PropertyType), prop.PropertyType))
        Next
        For Each item As T In data
            Dim row As DataRow = table.NewRow()
            For Each prop As PropertyDescriptor In properties
                row(prop.Name) = If(prop.GetValue(item), DBNull.Value)
            Next
            table.Rows.Add(row)
        Next
        Return table

    End Function

    '=======================================================
    'Service provided by Telerik (www.telerik.com)
    'Conversion powered by NRefactory.
    'Twitter: @telerik
    'Facebook: facebook.com/telerik
    '=======================================================


    Public Shared Function GetSimpleSearchCount(ByVal SearchString As String, Optional ByVal PriceRangeID As Integer = 0, Optional ByVal VendorID As Integer = 0) As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@SearchString", SearchString))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        Return DatabaseClasses.DatabaseConnect.QueryDb("Items_SimpleSearchCount", paramrry)
    End Function

    Public Shared Function GetQuickAdd(ByVal ItemNumber As String) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetQuickAdd", paramrry)
        Return dt
    End Function


    Public Function GetByID() As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim ri As New RedisInstance()
        Dim dt As New DataTable
        Dim serializedString As String
        Dim deserializedObject As String
        Dim keyRedis As String = "Items" & ItemID & ":" & GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        If Not ri.CheckRedisKey(keyRedis) Then
            dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetbyID", paramrry)
            serializedString = ri.Serialize(dt)
            ri.SetKey(keyRedis, serializedString)
        Else
            serializedString = ri.GetKey(keyRedis)
            deserializedObject = ri.Deserialize(Of String)(serializedString)
            dt = ri.Deserialize(Of DataTable)(deserializedObject)
        End If
        If dt.Rows.Count > 0 Then
            ThumbnailPath = dt.Rows(0)("ThumbnailPath")
            ImagePath = dt.Rows(0)("ImagePath")
            ItemName = dt.Rows(0)("ItemName")
            RetailPrice = GlobalFunctions.CheckNumeric_Decimal(dt.Rows(0)("RetailPrice"))
            ShortDesc = dt.Rows(0)("ShortDesc")
            LongDesc = dt.Rows(0)("LongDesc")
            VendorID = dt.Rows(0)("VendorID")
            Active = dt.Rows(0)("Active")
            Featured = dt.Rows(0)("Featured")
            Archive = dt.Rows(0)("Archive")
            ItemNumber = dt.Rows(0)("ItemNumber")
            CategoryID = dt.Rows(0)("CategoryID")
            SubCategoryID = dt.Rows(0)("SubCategoryID")
            PageTitle = GlobalFunctions.checkdbnull(dt.Rows(0).Item("PageTitle"))
            MetaTag = GlobalFunctions.checkdbnull(dt.Rows(0).Item("MetaTag"))
            Keywords = GlobalFunctions.checkdbnull(dt.Rows(0).Item("Keywords"))
            PageTitleDefault = GlobalFunctions.checkdbbit(dt.Rows(0).Item("PageTitleDefault"))
            MetaTagDefault = GlobalFunctions.checkdbbit(dt.Rows(0).Item("MetaTagDefault"))
            KeywordsDefault = GlobalFunctions.checkdbbit(dt.Rows(0).Item("KeywordsDefault"))
            Option1 = GlobalFunctions.checkdbbit(dt.Rows(0).Item("Option1"))
            Option2 = GlobalFunctions.checkdbbit(dt.Rows(0).Item("Option2"))
            SpecialPrice = GlobalFunctions.checkdbnull(dt.Rows(0).Item("SpecialPrice"))
            SpecialID = GlobalFunctions.checkdbinteger(dt.Rows(0).Item("SpecialID"))
            SellingPrice = GlobalFunctions.CheckNumeric_Decimal(dt.Rows(0).Item("SellingPrice"))
            Weight = GlobalFunctions.CheckNumeric_Decimal(dt.Rows(0).Item("Weight"))
            ManufacturerPartNumber = dt.Rows(0)("ManufacturerPartNumber")
            ProductLineID = GlobalFunctions.checkdbinteger(dt.Rows(0).Item("ProductLineID"))
            CategoryIDLevel3 = GlobalFunctions.checkdbinteger(dt.Rows(0).Item("CategoryIDLevel3"))
            CategoryIDLevel4 = GlobalFunctions.checkdbinteger(dt.Rows(0).Item("CategoryIDLevel4"))
            ThumbnailURL = GlobalFunctions.checkdbnull(dt.Rows(0).Item("ThumbnailURL"))
            LargeImageURL = GlobalFunctions.checkdbnull(dt.Rows(0).Item("LargeImageURL"))
            UNSPSCCode = GlobalFunctions.checkdbnull(dt.Rows(0).Item("UNSPSCCode"))
        End If
        Return dt

    End Function

    Public Shared Sub PopulateItems(ByRef ddl As System.Web.UI.WebControls.DropDownList)
        Dim dt As New DataTable
        dt = GetItems("", "1", "", "")
        GlobalFunctions.PopulateDropDownWithDataTable(dt, "ItemName", "ItemID", ddl, True)
    End Sub

    Public Shared Function GetDiscount(ByVal ItemID As Integer, Optional ByVal Qty As Integer = 0)
        Dim p As New ArrayList
        p.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        p.Add(New SqlClient.SqlParameter("@Qty", Qty))
        Return DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetDiscount", p)
    End Function

    Public Shared Function GetDiscountItems(ByVal DiscountGroupID As Integer)
        Dim p As New ArrayList
        p.Add(New SqlClient.SqlParameter("@DiscountGroupID", DiscountGroupID))
        Return DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetDiscountItems", p)
    End Function

    Public Shared Sub UpdateDiscountGroupID(ByVal DiscountGroupID As Integer, ByVal ItemID As Integer)
        Dim p As New ArrayList
        p.Add(New SqlClient.SqlParameter("@DiscountGroupID", DiscountGroupID))
        p.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        DatabaseClasses.DatabaseConnect.QueryDb("Items_UpdateDiscountGroupID", p)
    End Sub

    Public Shared Function GetItemsNotAssociatedList(ByVal DiscountGroupID As Integer, Optional ByVal ItemName As String = "", Optional ByVal ItemNumber As String = "")
        Dim p As New ArrayList
        p.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        p.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        p.Add(New SqlClient.SqlParameter("@DiscountGroupID", DiscountGroupID))
        Return DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetItemsNotAssociatedWithDiscountGroups", p)
    End Function

    Public Shared Function GetBySpecialID(ByVal SpecialID As Integer, Optional ByVal Active As Integer = -1, Optional ByVal Current As Integer = -1, Optional ByVal PriceRangeID As Integer = 0, Optional ByVal VendorID As Integer = 0, Optional ByVal PageIndex As Integer = 0, Optional ByVal NumRows As Integer = 0) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@SpecialID", SpecialID))
        paramrry.Add(New SqlClient.SqlParameter("@Active", Active))
        paramrry.Add(New SqlClient.SqlParameter("@Current", Current))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@VendorID", VendorID))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))

        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next

        Dim ri As New RedisInstance()
        Dim dt As New DataTable

        Dim startRowIndex As Integer = PageIndex * NumRows + 1
        Dim endRowIndex As Integer = startRowIndex + NumRows - 1
        Dim serializedString As String
        Dim deserializedObject As String
        Dim keyRedis As String = "Items" & VendorID & ":" & PriceRangeID & ":" & GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID")) & ":" & SpecialID
        If Not ri.CheckRedisKey(keyRedis) Then
            dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetBySpecialIDAll", paramrry)
            serializedString = ri.Serialize(dt)
            ri.SetKey(keyRedis, serializedString)
            dt = FilterDataTableRow(dt, startRowIndex, endRowIndex)
        Else
            serializedString = ri.GetKey(keyRedis)
            deserializedObject = ri.Deserialize(Of String)(serializedString)
            dt = ri.Deserialize(Of DataTable)(deserializedObject)
            dt = FilterDataTableRow(dt, startRowIndex, endRowIndex)
        End If
        Return dt
    End Function
    Public Shared Function GetForExport(ByVal ItemName As String, ByVal Active As String, ByVal ItemNumber As String, ByVal CategoryName As String, Optional ByVal ManufacturerPartNumber As String = "") As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        'paramrry.Add(New SqlClient.SqlParameter("@CategoryID", CategoryID))
        'paramrry.Add(New SqlClient.SqlParameter("@SubCategoryID", SubCategoryID))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryName", CategoryName))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetForExport", paramrry)
        Return dt
    End Function
    Public Shared Function AdvancedSearchItems(Optional ByVal ManufacturerPartNumber As String = "", Optional ByVal ItemNumber As String = "", Optional ByVal ProductLine As String = "", Optional ByVal Keywords As String = "", Optional ByVal Vendor As String = "", Optional ByVal OrderBy As String = "", Optional ByVal PageIndex As Integer = -1, Optional ByVal NumRows As Integer = 0, Optional ByVal PriceRangeID As Integer = 0, Optional ByVal SearchAll As Integer = 0) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ProductLine", ProductLine))
        paramrry.Add(New SqlClient.SqlParameter("@Keywords", Keywords))
        paramrry.Add(New SqlClient.SqlParameter("@Vendor", Vendor))
        paramrry.Add(New SqlClient.SqlParameter("@OrderBy", OrderBy))
        paramrry.Add(New SqlClient.SqlParameter("@PageIndex", PageIndex))
        paramrry.Add(New SqlClient.SqlParameter("@NumRows", NumRows))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@All", SearchAll))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        'For Each p As SqlClient.SqlParameter In paramrry
        '    System.Web.HttpContext.Current.Response.Write(p.ParameterName & " " & p.Value & "</br>")
        'Next
        'System.Web.HttpContext.Current.Response.End()
        'Dim dt As New DataTable
        'dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_AdvancedSearch", paramrry)
        Dim dt As New DataTable

        Dim node As New Uri("http://localhost:9200")

        Dim settings As New ConnectionSettings(node)

        Dim client As New ElasticClient(settings)

        Dim CID = GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))
        Dim results
        Dim sortField = "itemID"
        Dim sOrder = 0


        If Not OrderBy = "" Then

            sortField = OrderBy.Split(" ")(0).Substring(0, 1).ToLower() & OrderBy.Split(" ")(0).Substring(1)
            If OrderBy.Split(" ")(1) = "ASC" Then
                sOrder = 0
            Else
                sOrder = 1
            End If


        End If


        If SearchAll = 1 Then
            results = client.
                        Search(Of NetaMapping)(Function(x) x.Index("esdata").
                        Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.
                        Should(Function(s) s.Term("manufacturerPartNumber", ManufacturerPartNumber), Function(g) g.Term("manufacturerPartNumberClean", ManufacturerPartNumber),
                        Function(i) i.Term("itemNumber", ItemNumber), Function(i) i.Term("itemNumberClean", ItemNumber),
                        Function(pl) pl.Term("productLineName", ProductLine), Function(plc) plc.Term("productLineNameClean", ProductLine),
                        Function(k) k.Term("keywords", Keywords), Function(k) k.Term("keywordsClean", Keywords),
                        Function(k) k.Term("vendorName", Vendor.ToLower().Trim()), Function(k) k.Term("vendorClean", Vendor.ToLower().Trim())
                        ))).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf ManufacturerPartNumber IsNot "" Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").
                             Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.
                            Should(Function(s) s.Term("manufacturerPartNumber", ManufacturerPartNumber), Function(g) g.Term("manufacturerPartNumberClean", ManufacturerPartNumber)))).Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf ItemNumber IsNot "" Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").
                            Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.
                           Should(Function(s) s.Term("itemNumber", ItemNumber), Function(g) g.Term("itemNumberClean", ItemNumber)))).
                   Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf ProductLine IsNot "" Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").
                            Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.
                           Should(Function(s) s.Term("productLineName", ProductLine), Function(g) g.Term("productLineNameClean", ProductLine)))).
                   Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        ElseIf Keywords IsNot "" Then
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").
                            Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.
                           Should(Function(s) s.Term("keywords", Keywords.ToLower().Trim()), Function(g) g.Term("keywordsClean", Keywords.ToLower().Trim())))).
                   Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))
        Else
            results = client.Search(Of NetaMapping)(Function(x) x.Index("esdata").Query(Function(d) d.Nested(Function(p) p.Path("customerGroupIDNested").Query(Function(q) q.Term("customerGroupIDNested.groupID", CID))) And d.Bool(Function(f) f.Should(Function(s) s.Term("vendorClean", Vendor.ToLower().Trim()), Function(y) y.Term("vendorName", Vendor.ToLower().Trim())))).
                                                        Sort(Function(u) u.Field("_score", SortOrder.Ascending)).Sort(Function(ss) ss.Field(sortField, sOrder)).From(PageIndex * 10).Size(NumRows))

        End If
        If results.Documents.Count > 0 Then
            For Each obj As NetaMapping In results.Documents
                obj.ProductCount = results.Total
            Next

        End If


        dt = ConvertToDataTable(results.Documents)

        Return dt
    End Function

    Public Shared Function GetAdvancedSearchCount(Optional ByVal ManufacturerPartNumber As String = "", Optional ByVal ItemNumber As String = "", Optional ByVal ProductLine As String = "", Optional ByVal Keywords As String = "", Optional ByVal Vendor As String = "", Optional ByVal PriceRangeID As Integer = 0, Optional ByVal SearchAll As Integer = 0) As Integer
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@ProductLine", ProductLine))
        paramrry.Add(New SqlClient.SqlParameter("@Keywords", Keywords))
        paramrry.Add(New SqlClient.SqlParameter("@Vendor", Vendor))
        paramrry.Add(New SqlClient.SqlParameter("@PriceRangeID", PriceRangeID))
        paramrry.Add(New SqlClient.SqlParameter("@All", SearchAll))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Return DatabaseClasses.DatabaseConnect.QueryDb("Items_AdvancedSearchCount", paramrry)
    End Function

    Public Shared Function GetFixedPriceItems(ByVal ItemName As String, ByVal Active As String, ByVal ItemNumber As String, ByVal CategoryName As String, Optional ByVal ManufacturerPartNumber As String = "", Optional ByVal CustomerGroupID As Integer = -1) As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@ItemName", ItemName))
        paramrry.Add(New SqlClient.SqlParameter("@Status", Active))
        paramrry.Add(New SqlClient.SqlParameter("@ItemNumber", ItemNumber))
        paramrry.Add(New SqlClient.SqlParameter("@CategoryName", CategoryName))
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", If(CustomerGroupID > -1, CustomerGroupID, GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID")))))
        paramrry.Add(New SqlClient.SqlParameter("@ManufacturerPartNumber", ManufacturerPartNumber))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetFixedPriceItems", paramrry)
        Return dt
    End Function

    Public Shared Sub MoveToItems()
        Dim p As New ArrayList
        DatabaseClasses.DatabaseConnect.QueryDb("tmpItems_MoveToItems", p)
    End Sub

    Public Shared Sub BulkInsertImport(ByVal dt As DataTable)
        Dim con As System.Data.SqlClient.SqlConnection = DatabaseClasses.DatabaseConnect.OpenConnection()

        Dim bulkCopy As New System.Data.SqlClient.SqlBulkCopy(con)
        bulkCopy.DestinationTableName = "tmpItems"
        bulkCopy.BulkCopyTimeout = 600

        For Each c As DataColumn In dt.Columns
            bulkCopy.ColumnMappings.Add(c.ColumnName, c.ColumnName)
        Next

        bulkCopy.WriteToServer(dt)

        con.Close()

    End Sub

    Public Shared Function GetFeatured() As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_GetFeatured", paramrry)
        Return dt
    End Function

    Public Shared Function GetFeaturedItems() As DataTable
        Dim paramrry As New ArrayList
        paramrry.Add(New SqlClient.SqlParameter("@CustomerGroupID", GlobalFunctions.checkdbinteger(System.Web.HttpContext.Current.Session("CustomerGroupID"))))
        Dim dt As New DataTable
        dt = DatabaseClasses.DatabaseConnect.GetDataTable("Items_Featured_GetList", paramrry)
        Return dt
    End Function

    Public Shared Sub UpdatePricing(Optional ByVal ItemID As Integer = 0)
        Dim p As New ArrayList
        p.Add(New SqlClient.SqlParameter("@ItemID", ItemID))
        DatabaseClasses.DatabaseConnect.QueryDb("Items_UpdatePricing", p)
    End Sub
End Class
