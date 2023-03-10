# How-to-achieve-the-EditorSelectionBehavior-as-SelectAll-in-GridTemplateColumn-in-WinUI-DataGrid-SfD

The [EditorSelectionBehavior](https://help.syncfusion.com/cr/winui/Syncfusion.UI.Xaml.Grids.SfGridBase.html#Syncfusion_UI_Xaml_Grids_SfGridBase_EditorSelectionBehavior) as [SelectAll](https://help.syncfusion.com/cr/winui/Syncfusion.UI.Xaml.Grids.EditorSelectionBehavior.html#Syncfusion_UI_Xaml_Grids_EditorSelectionBehavior_SelectAll) will not work when the [GridTemplateColumn](https://help.syncfusion.com/cr/winui/Syncfusion.UI.Xaml.DataGrid.GridTemplateColumn.html) is in edit mode in [WinUI DataGrid](https://www.syncfusion.com/winui-controls/datagrid) (SfDataGrid). Because the element loaded inside the edit template cannot be predicted. You can achieve this by programmatically selecting all the text whenever the edit element got focused.

In the following sample, **TextBox** has been loaded as an edit element of **GridTemplateColumn** and all the text has been selected which is present inside the TextBox by setting the TextBox custom property **AutoSelectable** to true.

Refer to the following code snippet to achieve the **EditorSelectionBehavior** as **SelectAll** in **GridTemplateColumn**.

**XAML**

```XML
<Syncfusion:SfDataGrid x:Name="dataGrid"  
                       ItemsSource="{Binding Orders,Mode=TwoWay}"
                       AllowFiltering="True"
                       EditorSelectionBehavior="SelectAll"
                       ShowGroupDropArea="True"
                       AutoGenerateColumns="False"
                       AllowEditing="True">
    <Syncfusion:SfDataGrid.Columns>
        <Syncfusion:GridNumericColumn  MappingName="OrderID"    HeaderText="Order ID"     />
        <Syncfusion:GridTemplateColumn MappingName="CustomerID">
            <Syncfusion:GridTemplateColumn.CellTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding CustomerID}" />
                </DataTemplate>
            </Syncfusion:GridTemplateColumn.CellTemplate>
            <Syncfusion:GridTemplateColumn.EditTemplate>
                <DataTemplate>
                    <local:CustomTextBox AutoSelectable="True"  Text="{Binding CustomerID, Mode=TwoWay}"  />
                </DataTemplate>
            </Syncfusion:GridTemplateColumn.EditTemplate>
        </Syncfusion:GridTemplateColumn>
        <Syncfusion:GridTextColumn     MappingName="ShipCity"   HeaderText="Ship City"    />
        <Syncfusion:GridNumericColumn  MappingName="UnitPrice"  HeaderText="Unit Price"   />
        <Syncfusion:GridCheckBoxColumn MappingName="Review"     HeaderText="Review"  />
    </Syncfusion:SfDataGrid.Columns>
</Syncfusion:SfDataGrid>

```

**Code snippet related to CustomTextBox**

```C#

public class CustomTextBox : TextBox
{
    public CustomTextBox() : base()
    {

    }

    public static bool GetAutoSelectable(DependencyObject obj)
    {
        return (bool)obj.GetValue(AutoSelectableProperty);
    }

    public static void SetAutoSelectable(DependencyObject obj, bool value)
    {
        obj.SetValue(AutoSelectableProperty, value);
    }

    public static readonly DependencyProperty AutoSelectableProperty =
    DependencyProperty.RegisterAttached("AutoSelectable", typeof(bool), typeof(CustomTextBox), new PropertyMetadata(false, AutoFocusableChangedHandler));

    private static void AutoFocusableChangedHandler(DependencyObject d, DependencyPropertyChangedEventArgs e)
    {
        if (e.NewValue != e.OldValue)
        {
            if ((bool)e.NewValue == true)
            {
                (d as TextBox).GotFocus += OnGotFocusHandler;
            }
            else
            {
                (d as TextBox).GotFocus -= OnGotFocusHandler;
            }
        }
    }

    private static void OnGotFocusHandler(object sender, RoutedEventArgs e)
    {
        (sender as TextBox).SelectAll();
    }

}

```

Take a moment to peruse the [WinUI DataGrid - Column Types](https://help.syncfusion.com/winui/datagrid/column-types) documentation, to learn more about column types with code examples.

