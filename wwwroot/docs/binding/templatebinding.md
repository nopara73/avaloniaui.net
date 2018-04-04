Title: Binding in a Control Template
Order: 50
---

When you're creating a control template and you want to bind to the templated parent you can use:

```xml
<TextBlock Name="tb" Text="{TemplateBinding Caption}"/>

<!-- Which is short for -->
<TextBlock Name="tb" Text="{Binding Caption, RelativeSource={RelativeSource TemplatedParent}}"/>
```

This usage is identical to WPF/UWP etc.