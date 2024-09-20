## 프로젝트 설정
1. `Avalonia .NET MVVM App` 템플릿 선택
2. `고급설정` - `Usecompiled Bindings` 체크 X
3. `고급설정` - `Remove Avalonia ViewLocator` 체크 X
3. `MVVMToolkit` - CommunityToolkit 선택

## 현재 템플릿 라이브러리 버전 정보
1. Avalonia - 11.1.3
2. CommunityToolkit - 8.2.2
3. Projektanker.Icons.Avalonia - 9.4.0
3. Projektanker.Icons.Avalonia.MaterialDesign - 9.4.0

## Assets
1. BMW Logo.png

## 앱 리소스
```xml
<Application.Resources>
    <SolidColorBrush x:Key="ColorBlack">#252525</SolidColorBrush>
    <SolidColorBrush x:Key="ColorRed">#fd5b5a</SolidColorBrush>
    <SolidColorBrush x:Key="ColorWhite">#D3D3D3</SolidColorBrush>
</Application.Resources>
```
## 폴더트리
`Assets` 

`Components`

`Pages`

&nbsp;&nbsp; &nbsp;&nbsp;ㄴ`Bike`

&nbsp;&nbsp; &nbsp;&nbsp;ㄴ`Home`

&nbsp;&nbsp; &nbsp;&nbsp;ㄴ`RentalOffice`

`ViewModels`

## ViewLocator 클래스 수정
 기존 -> Views/{화면이름}View.xaml

 변경 -> Pages/{화면이름}/Index.xaml 

```csharp
public Control? Build(object? data)
{
    if (data is null)
        return null;
    var pathName = data.GetType().Namespace!.Replace("ViewModel", "Page");
    var folderName = data.GetType().Name!.Replace("ViewModel", "");
    // var name = data.GetType().FullName!.Replace("ViewModel", "View", StringComparison.Ordinal);
    var type = Type.GetType($"{pathName}.{folderName}.Index");

    if (type != null)
    {
        var control = (Control)Activator.CreateInstance(type)!;
        control.DataContext = data;
        return control;
    }
    
    return new TextBlock { Text = "Not Found: " + $"{pathName}.{folderName}.index" };
}

public bool Match(object? data)
{
    return data is ViewModelBase;
}
```

## ICON 라이브러리 사용하기 위해 수정
```csharp
using Avalonia;
using System;
using Projektanker.Icons.Avalonia;
using Projektanker.Icons.Avalonia.MaterialDesign;

namespace BicycleSharingSystemavaloniaTemplate;

sealed class Program
{
    // Initialization code. Don't use any Avalonia, third-party APIs or any
    // SynchronizationContext-reliant code before AppMain is called: things aren't initialized
    // yet and stuff might break.
    [STAThread]
    public static void Main(string[] args) => BuildAvaloniaApp()
        .StartWithClassicDesktopLifetime(args);

    // Avalonia configuration, don't remove; also used by visual designer.
    public static AppBuilder BuildAvaloniaApp()
    {
        IconProvider.Current
            .Register<MaterialDesignIconProvider>();
        
        return AppBuilder.Configure<App>()
            .UsePlatformDetect()
            .WithInterFont()
            .LogToTrace();
    }
}
```
