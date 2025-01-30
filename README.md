# (Some notes about Penrose tilings)

## Penrose (P3) "innerness"

Here is a Penrose P3 tiling produced by starting with a half-rhomb and recursively replacing as so:  
![image](https://github.com/user-attachments/assets/f7794a42-37ce-494f-ad09-3bc5746a1d2c)  
(todo: label vertices to show orientations)

After 5 iterations you get a result like this:  
![image](https://github.com/user-attachments/assets/0ca63960-c793-47e1-904c-cb8d07f113bf)
After 11 iterations:  
![image](https://github.com/user-attachments/assets/fddba588-cf5b-4fb7-abbb-b844c72b0407)

The "innerness" of each tile is calculated as part of the recursive replacement [(details)](https://math.stackexchange.com/questions/4579309/penrose-tiling-nesting-number), and that "innerness" determined the tile color in the pictures above. I haven't found any info about this "innerness" concept. I'm surprised, because it looks quite nice!

My vague goal is to learn more about innerness. Can it be calculated more directly? Can it be calculated using other Penrose construction methods? 

## Ammann Bars

If we draw some specific line segments on each fat & skinny rhombus, some Ammann bars emerge:  
![image](https://github.com/user-attachments/assets/12adf422-081c-4e87-bdee-882f7512b9e1)  
![image](https://github.com/user-attachments/assets/d1fce4b0-8df7-4d46-a52c-c3793c29c362)  

(There is a lot of good informations online about these Ammann bars.)  

A set of Ammann bars uniquely defines a Penrose tiling. Can we directly the compute Ammann bars in the Penrose tiling I've been generating using the recursive-replacement method?

Yes!
![image](https://github.com/user-attachments/assets/a3f5c4c2-6257-45f7-a853-13940b06958b)  

In the image above, there are two vertical red lines, whose positions can be calculated. The two lines form a "long" (L) Ammann bar (which is $\phi$ times wider than a "short" (S) Ammann bar).

How do the Ammann bars change as the recursive depth is increase?  
![image](https://github.com/user-attachments/assets/b9ef8e69-447b-4c1e-8b37-69b9e5fb1353)  

at each iteration:  
```
L -> [½L]S[½L]  
S -> [½L][½L]  
```

so, after two iterations:  
```
L -> [½S]L[½S]  
S -> [½S]LL[½S]  
```

so, after three iterations:  
```
L -> LSLSL  
S -> LSL  
```

Note that the Ammann lines will only be reused after 3 recursive iterations.

## 5 sets of Ammann Bars

So, we computed the set of Ammann bars for one orientation. We can take a shortcut (with caveats) for the other orientations.

Note that the tiling (and Ammann bars) are symmetrical about the green dot (see image below). At least locally. So, let's just rotate our Ammann bars about this green dot to generate the Ammann bars for the other 4 orientations.  
![image](https://github.com/user-attachments/assets/a2345744-5544-4f48-a4b0-d12af8d448a8)  


