1. Finish the WPF tutorial in this website: https://wpf-tutorial.com

1. Start with the basic controls : TextBox, Button, TextBlock and others. Understand what properties are exposed by those controls and how are they useful.
2. Play with the layout controls like : Grid, StackPanel, DockPanel etc.,. Spend a lot of time on doing this. Learn to create complex UI designs.
3. Learn about looping controls like ItemsControl.  
    
4. Learn more about templates. How to modify controls like having a checkbox in combobox, having an image in button, how to have different templates in ItemsControl etc etc.
5. Understand how data binding works. Start to create a MVVM or similar kind of application.
6. Create custom controls. Explore DependencyProperties and AttachedProperties.
7. Create a style resource and understand how to style the controls.

Next, you can do a sample project.

1) Start with binding a collection to Data grid.

2) Learn about the class INotifyPropertyChanged.  
[How to: Implement Property Change Notification](https://msdn.microsoft.com/en-us/library/vstudio/ms743695(v=vs.100).aspx "msdn.microsoft.com")

3) Learn about Observable collection. They have a very wide usage in terms of bindings and notifying the changes you make in the back end.

4)Make the columns on the grid editable. Replace the columns with text controls(user item template). Create a property for every column that captures the text change event. Use types of binding on the text controls. Try to capture the changes you make on the grid at the back end.  
[ListView, data binding and ItemTemplate](http://www.wpf-tutorial.com/listview-control/listview-data-binding-item-template/ "www.wpf-tutorial.com")  
[WPF Data Binding - Part 1](http://www.codeproject.com/Articles/29054/WPF-Data-Binding-Part "www.codeproject.com")

5)Once you succeed binding the text controls in the data grid with the back end properties, create a replica of the grid on the same page. Try to sync these two grids. Example, every change you make in the first grid must automatically update on the second grid.
