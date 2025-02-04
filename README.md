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

## 5 sets of Ammann bars

So, we computed the set of Ammann bars for one orientation. We can take a shortcut (with caveats) for the other orientations.

Note that the tiling (and Ammann bars) are symmetrical about the green dot (see image below). At least locally. So, let's just rotate our Ammann bars about this green dot to generate the Ammann bars for the other 4 orientations.  
![image](https://github.com/user-attachments/assets/a2345744-5544-4f48-a4b0-d12af8d448a8)  

Now we can generate the Ammann bars "directly", and see that they exactly match the Ammann bars generated recursively (for the portion near the "green dot"):  
![image](https://github.com/user-attachments/assets/24c3eb26-c3a5-4194-b95b-ed09ffa6ca91)

## Computing innerness for the Ammann bars

I found a function that seems like a good candidate for computing the "innerness" of Ammann bars. In the image below, note "graph" at the bottom. Horizontally, each line segment corresponds to half an Ammann bar. The vertical position of the line segment corresponds to lighter/darker colors (and therefore innerness). The lower the segment, the brighter the tiling for the half-Ammann bar.

![image](https://github.com/user-attachments/assets/87fb97c7-f50f-4721-98cd-3af71700bcf8)

This function works similarly to how innerness is calculated for the Penrose tiling.

(todo: make this part clearer) 
Basically, this function recursively subdivides an Amman half-bar. When stepping into a short (S) Ammann bar, the "innerness" either increases or decreases by 1, depending on the parity of the recursive depth.
```
   // this can be simplified
   int calcInnerness( double t0, double tc, double x, bool isLong, int depth )
   {
      if ( depth == 0 )
         return 0;

      if ( isLong )
      {
         double tmid = mix( t0, tc, 1 / PHI );
            return (x > tmid) != (x > t0) ? calcInnerness( tmid, t0, x, true, depth - 1 )
                                          : calcInnerness( tmid, tc, x, false, depth - 1 ) + (depth & 1 ? 1 : -1);
      }
      else
      {
         return calcInnerness( tc, t0, x, true, depth - 1 );
      }
   };
```
or
```
   // x in [0, .5] on a long (L) Ammann bar
   int calcInnerness( double x, int depth )
   {
      if ( depth <= 0 )
         return 0;

      return x < .5 / PHI ? calcInnerness( .5 - x * PHI, depth - 1 )
         : calcInnerness( (.5 - x) / (2 - PHI), depth - 2 ) + (depth & 1 ? 1 : -1);
   };
```

Now for each point on the plane we can simply add up the innerness values of the corresponding half-Ammann bar in all 5 orientations, and get a result that matches the recursive, *very* roughly:  
![image](https://github.com/user-attachments/assets/76e4fbce-2577-46b8-af7c-5fe5c3372067)  
![image](https://github.com/user-attachments/assets/71d0ce75-4f87-4b7f-92d5-ede63f7153e4)  

## Computing tiles, given the Ammann bars

![image](https://github.com/user-attachments/assets/94b4fea8-d7f2-4fe8-ace4-c1a1d05b171b)

The fat and skinny tiles always have the same Ammann bar pattern on them. But, starting with the Ammann bars, how can we reconstruct the tiles in a simple way?  

There doesn't seem to be a simple way.

But, now let's draw the Ammann bars from the previous iteration instead:

![image](https://github.com/user-attachments/assets/2d91c7ed-2a5d-421e-931e-cad317e7112f)

Now, for every intersection of "steep" Ammann bar lines, there is a corresponding yellow rhombus. And for every instersection of "shallow" sloped Ammann bar line, there is a corresponding pink rhombus.

Notice that every intersection of the bar lines, there is a corresponding tile.

Here are all the bars drawn in one image:  
![image](https://github.com/user-attachments/assets/fad77cef-1a8b-4639-add7-a160b338aa69)  

