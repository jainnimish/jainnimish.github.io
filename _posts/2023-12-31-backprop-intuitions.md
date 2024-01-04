---
layout: post
title:  "Intuitions for Backpropogation"
---

Understanding the raw concept of backprop is relatively easy. However, implementing some of the functions in vectorized `numpy` code was very challenging. These are some of my notes to break the problems into subcategories and addressing them accordingly.

### Tip 1.
Instead of writing fully vectorized code from the get-go, consider breaking the function into its smallest "atomic" pieces. For doing so, I found diving sub-operations into these gates very helpful.

- Plus
- Exponentiation (`a^b`)
- Max
- Multiplication

Subtraction can be represented as `a + (-b)` and division as `a * b^-1`.

Basically, any "gate" that you know how you take the derivative of easily. Division gates are bad for this reason because applying the quotient rule is much harder than the product + power rule. The problem gets worse when the operation is done recursively.

If you are constantly changing the previous variable in whatever way, it is best to allocate a new intermediate variable to this result.

### Tip 2.
Shapes. Now that you know what operation the gate is performing, it's crucial to know what the inputs and outputs are. As a rule of thumb, make sure to always check the `shapes` of both the input and output. This is because sometimes Numpy performs implicit broadcasting operations automatically to compute the output of the gate. During backprop, you must account for this.

Additionally, knowing shapes is sort of a sanity check because you know what the shape of the resultant Tensor should be.

### Tip 3.
Know specifically what elements in your input array are affected by the operation. Not all operations are elementwise. Even if they are, they can apply to only select parts of the input. For example, the gate `max(0, x)`.

### Tip 4.
Work with smaller batches. Generalizing is easier than writing code for the entire expression. Often, I find that creating a toy example using only the first input from the dataset is easier to manage.

### Tip 5.
Weird abnormalities. Sometimes, it is not straightforward at all what the local derivative of an operation is. You even create a toy example and the problem remains unsolvable.

Usually, because you know the shapes must work out, you have a few options:

- Either its some kind of dot product.
- A sum of the upstream derivative or some other quantity ex. `upstream.sum()`.
- The input is being used in multiple places. Check your intermediate variables to find out where. Remember, if an input is branching out to many nodes, the contributions will **add** up during backpropogation.

### Tip 6.
Sometimes, rather than staring at the screen and trying to figure out the local derivative, you may be better off simply writing the expression for the intermediate variable on paper and solving for the local derivative. The derivatives of simple gates are usually just combinations of the general rules of Calculus I.

Drawing the computational graph for that intermediate variable might get you somewhere.


### Summary notes for self:
- What is this operation doing? Can I introduce more intermediate variables to represent the computation?
- What is the local derivative?
    - Shapes of input and output?
        - Did you perhaps forget `keepdims = True`?
    - Does this operation apply to select elements of the input?
    - Am I summing something during forward prop? Taking the mean? Draw a simple 1D case to understand how the mean and summation behaves.
        - If summing something during forward pass, during backward pass, the local derivative is just ones in the shape of the input. Then, you multiply it by the upstream derivative (chain rule).
        - If `a = b + c`, then `da/db = d(b + c)/db = db/db + dc/db = 1 + 0 = 1`
    - Input being used in multiple places?
    - How should I initialize my array? Of what shape?
    - Gradients add at branches.
- Remember to apply Chain Rule.
- Draw simplest possible toy example if stuck.
- What is the derivative asking? It wants to know how the output changes with respect to the input. Sometimes, it's helpful to refer back to this definition.

### Hehe.
- Dirty tricks
    - Is there any operation I can perform to get the resultant Tensor's derivative shape correct?
    - If all else fails, it's probably a matrix multiplication or summation of some quantity around some axis.
    - Run an autograd engine on your atomic expressions and compare gradients manually.
    - Try not to follow natural instincts and get the answer in this fashion.

Overally, backprop was quite hard for me initially. With time, you start to observe some of these behaviours and develop tricks for them. I would recommend trying to do backprop on actual loss functions or common layers. I found it hard to persist despite not getting the answer for 3-4 hours, but stick with it.

I found this [lecture](https://www.youtube.com/watch?v=q8SA3rM6ckI) by Andrej very useful.
