.. include:: include.rst

.. _wpf_concepts:


############
WPF Concepts
############

************
Data Binding
************

About Data Binding

*********
Templates
*********

About Control template and data template

Reference : `Template blog <https://www.c-sharpcorner.com/article/different-types-of-templates-in-wpf/>`_

*********
Resources
*********

Resources are objects which can be reused anywhere in a WPF application. For example styles, brushes, Bitmap images etc. can be used as resources in different places in your WPF application.

 list of place where your resources can be declared:

* As global application scope within the application **App.xaml** file.

* As Window level scope within the Resources property of the current Window.

* Within the Resources property of any FrameworkElement or FrameworkContentElement.

* Separate XAML resource file.

1. Static Resource - Static resources are the resources which you cannot manipulate at runtime. 
                     The static resources are evaluated only once by the element which refers them during the loading of XAML.

2. Dynamic Resource - Dynamic resources are the resources which you can manipulate at runtime and are evaluated at runtime.
                      If your code behind changes the resource, the elements referring resources as dynamic resources will also change.


**Implicit Styling**

Applying style to all the specific control (Button,Label etc) used all over the xaml.
This is inefficient because we might not need to apply same style for all control(button).
For that we have to override some of the settings in xaml.

.. code-block:: xaml
   :caption: Implicit example
        <Style TargetType="Button">
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="FontSize" Value="25"/>
            <Setter Property="Margin" Value="5"/>
        </Style>

        <Style TargetType="Label">
            <Setter Property="FontSize" Value="70"/>
        </Style>

**Explicit Styling**

To avoid the problem of overriding the property for some controls, we can use explicit styling as below,

.. code-block:: xaml
   :caption: Explicit example
        <SolidColorBrush x:Key="numbersColor" Color="#444444"/>
        <SolidColorBrush x:Key="operatorsColor" Color="Orange"/>
        <SolidColorBrush x:Key="foregroundColor" Color="White"/>

        <Style TargetType="Button" x:Key="numberButtonsStyle">
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="FontSize" Value="20"/>
            <Setter Property="Margin" Value="5"/>
            <Setter Property="Background" Value="{StaticResource numbersColor}"/>
        </Style>

        <Style TargetType="Button" x:Key="operatorButtonsStyle" BasedOn="{StaticResource numberButtonsStyle}">
            <Setter Property="Background" Value="{StaticResource operatorsColor}"/>
        </Style>

        <Style TargetType="Button" x:Key="additionalButtonsStyle" BasedOn="{StaticResource numberButtonsStyle}">
            <Setter Property="Background" Value="LightGray"/>
            <Setter Property="Foreground" Value="Black"/>
        </Style>

https://www.dotnetcurry.com/wpf/1142/resources-wpf-static-dynamic-difference#:~:text=Static%20Resource%20%2D%20Static%20resources%20are,during%20the%20loading%20of%20XAML.&text=Dynamic%20Resource%20%2D%20Dynamic%20resources%20are,and%20are%20evaluated%20at%20runtime

https://www.c-sharpcorner.com/article/static-dynamic-resources-in-wpf/