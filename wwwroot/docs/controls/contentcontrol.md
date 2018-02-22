Title: ContentControl
---
The [`ContentControl`](/api/Avalonia.Controls/ContentControl) displays data according to a
[data template](/docs/templates/datatemplate).

At its simplest, a `ContentControl` displays the data assigned to its
[`Content`](/api/Avalonia.Controls/ContentControl/4B02A756) property.

For example:

```xml
<ContentControl Content="Hello World!"/>
```

Will display the string "Hello World!". The `Content` property is the control's default property
and so the above example can also be written as:

```xml
<ContentControl>Hello World!</ContentControl>
```

If you assign a control to a `ContentControl` then it will display the control, for example:

```xml
<ContentControl>
  <Button>Click Me!<Button>
</ContentControl>
```

So far so uninteresting. Where `ContentControl` becomes useful is in tandem with 
[data binding](/docs/binding) and [data templates](/docs/templates/datatemplate). By setting the
[`ContentTemplate`](/api/Avalonia.Controls/ContentControl/ACED680E) property one can specify how
the data in the `Content` property is displayed. For example given the following view models:

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        object content = new Student("Jane", "Deer");

        public object Content
        {
            get => content;
            set => this.RaiseAndSetIfChanged(ref content, value);
        }
    }

    public class Student
    {
        public Student(string firstName, string lastName)
        {
            FirstName = firstName;
            LastName = lastName;
        }

        public string FirstName { get; }
        public string LastName { get; }
    }
}
```

> Note: The following examples assume an instance of `MainWindowVieModel` is assigned to the Window's
  `DataContext`. See [the section on `DataContext`](/docs/binding/datacontext) for more information.

We can display the student's first and last name in a `ContentControl` using the `ContentTemplate`
property:

```xml
<Window xmlns="https://github.com/avaloniaui">
  <ContentControl Content="{Binding Content}">
    <ContentControl.ContentTemplate>
      <DataTemplate>
        <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
          <TextBlock Grid.Row="0" Grid.Column="0">First Name:</TextBlock>
          <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding FirstName}"/>
          <TextBlock Grid.Row="1" Grid.Column="0">Last Name:</TextBlock>
          <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding LastName}"/>
        </Grid>
      </DataTemplate>
    </ContentControl.ContentTemplate>
  </ContentControl>
<Window>
```

<img class="doc-img" src="images/student-first-last-name.png">

The data template for the `Content` doesn't only come from the `ContentTemplate` property. If a 
`ContentTemplate` isn't specified, the control will search up the tree (and then `App.xaml`) trying
to find a matching data template, so the above example could be rewritten as:

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:local="clr-namespace:Example">
  <Window.DataTemplates>
    <DataTemplate DataType="{x:Type local:Student}">
      <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
        <TextBlock Grid.Row="0" Grid.Column="0">First Name:</TextBlock>
        <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding FirstName}"/>
        <TextBlock Grid.Row="1" Grid.Column="0">Last Name:</TextBlock>
        <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding LastName}"/>
      </Grid>
    </DataTemplate>
  </Window.DataTemplates>

  <ContentControl Content="{Binding Content}"/>
<Window>
```

Lets add another view model into the mix:

```csharp
namespace Example
{
    public class Teacher
    {
        public Teacher(string firstName, string lastName)
        {
            FirstName = firstName;
            LastName = lastName;
        }

        public string FirstName { get; }
        public string LastName { get; }
    }
}
```

Now we can add a separate data template for the `Teacher` type and depending on the type of object
in the `MainWindowViewModel.Content` property, the appropriate view will be displayed:

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:local="clr-namespace:Example">
  <Window.DataTemplates>

    <DataTemplate DataType="{x:Type local:Student}">
      <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
        <TextBlock Grid.Row="0" Grid.Column="0">First Name:</TextBlock>
        <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding FirstName}"/>
        <TextBlock Grid.Row="1" Grid.Column="0">Last Name:</TextBlock>
        <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding LastName}"/>
      </Grid>
    </DataTemplate>

    <DataTemplate DataType="{x:Type local:Teacher}">
      <Grid ColumnDefinitions="Auto,4,Auto">
        <TextBlock Grid.Row="0" Grid.Column="0">Professor</TextBlock>
        <TextBlock Grid.Row="0" Grid.Column="2" Text="{Binding LastName}"/>
      </Grid>
    </DataTemplate>

  </Window.DataTemplates>

  <ContentControl Content="{Binding Content}"/>
<Window>
```
