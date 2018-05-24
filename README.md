# Handling the Back Button in Windows 10 UWP Apps

The cool thing about Universal Windows Platform (UWP) apps is that they run on an assortment of devices, from PCs, tablets, and phones to (soon) Xboxes and HoloLens, among others. 
But as every hero knows, with great power comes great responsibility. One of the issues you’ll run into when writing a UWP app is how to handle the Back button. Windows phones have Back buttons, but other devices don’t. One way to handle the Back button on phones is to add a reference to Microsoft’s Mobile Extension SDK and write adaptive code that responds to HardwareButtons.BackPressed events.

Microsoft introduced the Windows.UI.Core.SystemNavigationManager class. SystemNavigationManager does two important things for you:

1) Upon request, it displays a software Back button when your app is running on a device without a Back button
2) It allows you to handle clicks of the Back button (software or hardware) without adding extension SDKs and without writing adaptive code.

The beauty of it is that this code works without change on phones and other devices. SystemNavigationManager is smart enough not to display a software Back button on devices that have Back buttons, and to fire BackRequested events when the user clicks the software Back button provided by SystemNavigationManager or taps the hardware Back button on a phone.

So far, so good. But here’s where it gets interesting. How do you leverage SystemNavigationManager in a multipage app without adding SystemNavigationManager code to every page? Is there a way, for example, to centralize the Back-button code so that it “just works” independent of how many pages the app has or the content of those pages?

The answer is a resounding yes. In fact, there’s a pattern I use in the UWP apps I write that isolates all the SystemNavigationManager code in App.xaml.cs and works exactly as you’d expect it to. The Back button appears whenever the backstack contains a page to go back to, and disappears when it does not. All you have to do is add a few lines of code to the code already in App.xaml.cs.
