# Why Does the Root Locus Start at Open-Loop Poles?
If you’ve ever studied control systems, you’ve probably come across root locus diagrams. They’re plots that show how the system poles move as you change the gain  $K$ . But why does the root locus always start at the open-loop poles when  $K = 0$ ? Let’s break it down in simple terms.

When you analyze a feedback system, there’s this key equation called the “characteristic equation.” It looks like this:  $1 + K H(s) G(s) = 0$ . it’s just a fancy way of saying, “This is what determines the behavior of the system.” Here,  $G(s)$  is the forward transfer function,  $H(s)$  is the feedback transfer function, and  K  is the gain you’re adjusting.

To simplify things, people often write  $H(s) G(s)$  as a fraction:  $\frac{b(s)}{a(s)}$ . The top part,  $b(s)$ , relates to the zeros of the system, and the bottom part,  $a(s)$ , relates to the poles. Plugging this into the characteristic equation gives us  $a(s) + K b(s) = 0$ .

Now, here’s the key part. When  $K = 0$ , the equation simplifies to  $a(s) = 0$ . This means the roots of the system (aka, where the root locus starts) are just the poles of the open-loop transfer function. So, no matter how complicated the system is, the root locus will always start at those open-loop poles when  $K = 0$ .

![rootlocuscarequation](https://i.imgur.com/9xPRg5h.png)

As you increase  $K$ , something cool happens. The roots start moving away from those open-loop poles and head toward the system’s zeros. If there aren’t enough zeros, the remaining branches of the root locus will shoot off to infinity. That’s why root locus diagrams look like a bunch of branches stretching across the graph.

Why does this matter? Well, the location of the poles affects how the system behaves—whether it’s stable, how fast it responds, or how much it oscillates. By adjusting  $K$ , you’re basically steering those poles to get the performance you want. 

So the root locus starts at the open-loop poles because, at  $K = 0$ , Simple as that!
