# How to change the selected item background in Xamarin.Forms ListView (SfListView)

You can change the [BackgroundColor](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.visualelement.backgroundcolor) of the elements in the [ItemTemplate](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~ItemTemplate.html?) based on selection in Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview? using [converter](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/data-binding/converters).

You can also refer the following article.

https://www.syncfusion.com/kb/11543/how-to-customize-the-background-color-of-selected-itemtemplate-elements-in-xamarin-forms

**XAML**

Define the converter for [Grid](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/layouts/grid) and [Label](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/text/label) inside **ItemTemplate** to change the **BackgroundColor** when the item is selected using [SelectionChangedCommand](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~SelectionChangedCommand.html?) of **ListView**.
``` xml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewXamarin"
             xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms"
             x:Class="ListViewXamarin.MainPage">
    <ContentPage.BindingContext>
        <local:ContactsViewModel/>
    </ContentPage.BindingContext>
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:ColorConverter x:Key="colorConverter"/>
        </ResourceDictionary>
    </ContentPage.Resources>
  <ContentPage.Content>
        <StackLayout>
            <syncfusion:SfListView x:Name="listView" ItemSize="60" ItemsSource="{Binding ContactsInfo}" SelectionChangedCommand="{Binding ListViewSelectionChanged}">
                <syncfusion:SfListView.ItemTemplate>
                    <DataTemplate>
                        <Grid x:Name="grid" BackgroundColor="{Binding IsSelected, Converter={StaticResource colorConverter}, ConverterParameter={x:Reference grid}}">
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="70" />
                                <ColumnDefinition Width="*" />
                            </Grid.ColumnDefinitions>
                            <Image Source="{Binding ContactImage}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="50" WidthRequest="50"/>
                            <Grid Grid.Column="1" RowSpacing="1" Padding="10,0,0,0" VerticalOptions="Center">
                                <Label x:Name="label1" LineBreakMode="NoWrap" TextColor="#474747" Text="{Binding ContactName}" BackgroundColor="{Binding IsSelected, Converter={StaticResource colorConverter}, ConverterParameter={x:Reference label1}}"/>
                                <Label x:Name="label2" Grid.Row="1" Grid.Column="0" TextColor="#474747" LineBreakMode="NoWrap" Text="{Binding ContactNumber}" BackgroundColor="{Binding IsSelected, Converter={StaticResource colorConverter}, ConverterParameter={x:Reference label2}}"/>
                            </Grid>
                        </Grid>
                    </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>
            </syncfusion:SfListView>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```
**C#**

Change the **IsSelected** property of item based on selection using the [SelectionChangedCommandParameter](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~SelectionChangedCommandParameterProperty.html?).
``` c#
namespace ListViewXamarin
{
    public class ContactsViewModel : INotifyPropertyChanged
    {
        public Command ListViewSelectionChanged { get; set; }
        Contacts previousItem = null;
 
        public ContactsViewModel()
        {
            ListViewSelectionChanged = new Command(OnSelectionChanged);
        }
 
        private void OnSelectionChanged(object obj)
    {
        foreach (var selectedItem in (obj as Syncfusion.ListView.XForms.ItemSelectionChangedEventArgs).AddedItems)
            (selectedItem as Contacts).IsSelected = true;
 
        foreach (var removedItem in (obj as Syncfusion.ListView.XForms.ItemSelectionChangedEventArgs).RemovedItems)
            (removedItem as Contacts).IsSelected = false;
    }
    }
}
```
**C#**

Converter to change the **BackgroundColor** of the **ItemTemplate** elements.
``` c#
namespace ListViewXamarin
{
    public class ColorConverter : IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            return (bool)value ? parameter as Grid != null ? Color.Blue : Color.Yellow : Color.Transparent;
        }
 
        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
        {
            throw new NotImplementedException();
        }
    }
}
```
**Output**

![ElementSelection](https://github.com/SyncfusionExamples/item-template-elements-selection-listview-xamarin/blob/master/ScreenShots/ElementSelection.gif)
