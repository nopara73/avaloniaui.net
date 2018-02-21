Title: The DataContext
Order: 0
---
The [`Control.DataContext`](http://avaloniaui.net/api/Avalonia.Controls/Control/D29AE9A9) property
describes where controls will look by default for values when binding. The data context will usually
be set for top-level controls such as [`Window`](http://avaloniaui.net/api/Avalonia.Controls/Window)
and child controls will inherit this data context.

If you created your application with the [Avalonia MVVM Application](create-new-project.md) template
then you will see something like this in your `Program.cs` file:

```csharp
static void Main(string[] args)
{
    BuildAvaloniaApp().Start<MainWindow>(() => new MainWindowViewModel());
}
```

What this piece of code means is that when the `MainWindow` is created, a new instance of
`MainWindowViewModel` will be created and assigned to the window's `DataContext` property. From here
all bindings will by default bind to properties on this object.

You can bind child controls to properties on the `DataContext` too, for example:

```xml
<Window>
  <Button DataContext="{Binding ChildProperty}">
</Window>
```

Will bind the `Button`'s `DataContext` to `Window.DataContext.ChildProperty`.

Some controls automatically bind child controls' data contexts, for example 
[`ContentControl`](/docs/controls/contentcontrol) and [`ItemsControl`](/docs/controls/itemscontrol).