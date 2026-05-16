
### 1. Continuous Functions (No Jumps or Breaks)

In simple terms, a function is **continuous** if you can draw its graph without ever lifting your pen from the paper.
- **The Math Translation:** The text explains this by saying that if you make a very tiny change to the input ($\Delta t \rightarrow 0$), it will only result in a very tiny change to the output ($\Delta g \rightarrow 0$). There are no sudden leaps or teleportations from one value to another.
### 2. Differentiable Functions (Smooth Curves)

A function is **differentiable** if you can calculate its exact rate of change (its slope or "derivative") at any given point. Visually, this means the graph is completely smooth and has no sharp, pointy corners.

- **The Math Translation:** The text shows this with the limit formula $\lim_{\Delta t \rightarrow 0} \frac{\Delta g(t)}{\Delta t} = C$. This just means that as you zoom in closer and closer to a point, the slope settles on a specific, constant number ($C$).

### 3. The Golden Rule: Smoothness vs. Connectedness

The top of the second page highlights a crucial rule in calculus: **"Differentiability implies continuity, but not the other way around."**

- If a line is perfectly smooth (differentiable), it _must_ be connected (continuous).
- However, a line can be connected (continuous) but still have sharp corners where you can't calculate a single slope (not differentiable).

**The Examples:**

- **Example 1.1 ($g(t) = \sqrt{t}$):** This curve is connected, but right at the starting point ($t=0$), the line goes completely vertical. Because a vertical line has an "infinite" slope, it is not differentiable at that exact point.
    
- **Example 1.2 (The Weierstrass Function):** This is a famous, crazy mathematical curve. It is essentially a jagged fractal. It is connected everywhere (you never have to lift your pen), but it is so infinitely jagged and spiky that it is smooth _nowhere_. Therefore, it is continuous everywhere, but differentiable nowhere.
    

### 4. The Mean Value Theorem

This is a famous theorem (Theorem 1.1) that is easiest to understand with a driving analogy.

- **The Analogy:** If you drive 60 miles in exactly 1 hour, your _average_ speed is 60 mph. The Mean Value Theorem guarantees that at least once during your trip, your speedometer must have read _exactly_ 60 mph.
    
- **The Math Translation:** If a curve is connected and smooth between point A and point B, there is at least one point ($c$) in the middle where the actual slope at that exact moment is identical to the average slope between A and B.
    

### 5. Right- and Left-Continuous Functions

Sometimes, graphs _do_ have breaks or "steps" in them. This section introduces a way to describe what happens right at the edge of a broken step.

- Imagine tracing a graph with your finger. If you trace it from the left side approaching a specific point, you might end up at a different value than if you traced it from the right side.

- A function is **left-continuous** if the path from the left leads exactly to the actual point. It's **right-continuous** if the path from the right leads exactly to the actual point.
    
- If both paths meet up perfectly at the exact same point, the function is fully continuous.


In the real world, things aren't always smooth curves. Prices crash, shocks happen, and paths get jagged. These pages are setting up the mathematical rules for handling those jagged realities.

### 1. The Two Types of "Breaks" (Discontinuities)

Imagine walking along a path representing a mathematical function. Sometimes, the path breaks. The text categorizes these breaks into two kinds:

- **Discontinuity of the First Kind (A "Jump"):** Imagine walking to the edge of a gap. You look down, and the path continues exactly 5 feet below you. You know exactly where you stood just before the gap (the left limit), and you know exactly where the new path starts (the right limit). There is a clear, measurable jump.
    
- **Discontinuity of the Second Kind (The "Crazy" Break):** Example 1.3 gives the function $sin(1/t)$. Imagine walking toward $t=0$. As you get closer, the path starts vibrating up and down infinitely fast. By the time you reach $0$, the path is a blur. You don't have a clear "edge" to stand on, so you can't measure a simple jump.
    

**The Takeaway:** Stochastic calculus completely throws out "second kind" breaks. They are too messy. It only deals with functions that are either smooth or have clean, measurable jumps (First Kind).

### 2. The "Càdlàg" Function (Modeling Reality)

Page 4 introduces the term **càdlàg**. It’s an abbreviation of a French phrase meaning _"Right-continuous with left limits."_This is the MVP (Most Valuable Player) of financial mathematics.

Why is this specific shape so important? It perfectly models a sudden event, like a stock price reacting to news.

- **Left Limits:** Imagine a stock is trading smoothly at $10. Just _before_ 2:00 PM, the price is clearly approaching $10. You can track its history up to that exact microsecond.
    
- **Right-Continuous:** At exactly 2:00 PM, a CEO resigns. The stock instantly drops to $8. Because it is "right-continuous," it means that at exactly 2:00 PM, the price is $8, and it will continue smoothly from $8 for the next few moments.
    

**The Takeaway:** A càdlàg function establishes that when a sudden shock happens, the "new reality" takes hold instantly at that exact moment.

### 3. Total Variation (Measuring the "Wiggle")

Section 1.2 introduces the **Variation of a Function**.

Think of a function's graph plotted with a piece of string on a wall. If you pull that string perfectly straight and measure it with a ruler, how long is it? That concept is "variation." It measures the total vertical distance traveled.

- If a stock goes from $10 up to $15, and then drops back down to $10, its net change is $0.
- But its **variation** is $10 ($5 of upward movement + $5 of downward movement). It is the "odometer" of the function's vertical travel.

**The Takeaway:** If you can pull the string straight and it has a measurable length, it has **finite variation**. But here's the wild part: in stochastic calculus, true random processes (like "Brownian motion" used to model stock volatility) vibrate so violently up and down that if you tried to pull the string straight, it would be infinitely long! That's why traditional calculus breaks down, and they need this specific terminology to handle functions that never stop wiggling.