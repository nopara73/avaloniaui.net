Binding from code in Avalonia works somewhat differently to WPF/UWP. At the low level, Avalonia's
binding system is based on Reactive Extensions' `IObservable` which is then built upon by XAML
bindings (which can also be instantiated in code).

## Binding to an observable

You can bind a property to an observable using the `AvaloniaObject.Bind` method:

```csharp
// We use an Rx Subject here so we can push new values using OnNext
var source = new Subject<string>();
var textBlock = new TextBlock();

// Bind TextBlock.Text to source
textBlock.Bind(TextBlock.TextProperty, source);

// Set textBlock.Text to "hello"
source.OnNext("hello");
// Set textBlock.Text to "world!"
source.OnNext("world!");
```

## Setting a binding in an object initializer

It is often useful to set up bindings in object initializers. You can do this using the indexer:

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.ToBinding(),
};
```

Using this method you can also easily bind a property on one control to a property on another:

```csharp
var textBlock1 = new TextBlock();
var textBlock2 = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty],
};
```

Of course the indexer can be used outside object initializers too:

```csharp
textBlock2[!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty];
```

# Transforming binding values

Because we're working with observables, we can easily transform the values we're binding!

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.Select(x => "Hello " + x).ToBinding(),
};
```

# Using XAML bindings from code

Sometimes when you want the additional features that XAML bindings provide, it's easier to use XAML bindings from code. For example, using only observables you could bind to a property on `DataContext` like this:

```csharp
var textBlock = new TextBlock();
var viewModelProperty = textBlock.GetObservable(TextBlock.DataContext)
    .OfType<MyViewModel>()
    .Select(x => x?.Name);
textBlock.Bind(TextBlock, viewModelProperty);
```

However, it might be preferable to use a XAML binding in this case:

```csharp
var textBlock = new TextBlock
{
    [!TextBlock.TextProperty] = new Binding("Name")
};
```
