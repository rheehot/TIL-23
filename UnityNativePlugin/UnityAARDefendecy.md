DATE:2020.06.29.MON

While I am working on Unity native plugin development, I met a "ClassNotFoundException" case

 Similar Error Log

> com.unity3d.player.UnityPlayer$c.run(Unknown
 > Source:20) 02-05 22:45:56.281:
 > E/Unity(23488): Caused by:
 > java.lang.ClassNotFoundException:
 > Didn't find class

My project has a dependency like this

A: AAR in order to connect between Unity and Native
 
B: Some feature's AAR
  
c: A has a defendcy on B


Becasue When I build in Android studio to make an aar file, it was fine without any error and  with Implmention property in build gradle
I thought that it had been fine but 
It occured an error


 정답은 각각의 A,B AAR을 Unity Editor에 포함해 주어야 한다
 
 자세하게는 > Editor/Plugin/Android/A,B both aar files
