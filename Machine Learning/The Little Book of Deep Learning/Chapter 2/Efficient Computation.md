As of now, computation has been made so efficiency with the use of GPUs which have also been equipped with dedicated tensor cores and deep learning specialized chips such as the Google TPU.

Infact the efficiency and speed of this processors used for AI has been so much that most time is spent in waiting for the data to arrive, that is, the limiting factor or the slowest link is CPU to GPU.

The limiting factor is how fast the system can read and write data to memory.
In a typical machine learning setup, your main data lives in the standard system memory (CPU RAM), but the heavy math is done on the graphics card (GPU).

- Moving data across the bridge from the CPU to the GPU is incredibly slow compared to the processing speed.
- **The rule:** You want to move data across this bridge as infrequently as possible.
Even when the data arrives to the GPU, there are different levels of memory:
- **GPU Main Memory (VRAM):** Large but relatively slow.
- **Cache Memory:** Tiny storage areas placed physically close to the computing cores. They hold small amounts of data but are lightning-fast. To do math efficiently, data must be moved from the VRAM into these tiny, ultra-fast caches.

#### So what is the solution?
### Batch Processing
This is where the magic of "batches" comes in. An AI model requires two things to do its math:
1. **Model Parameters:** The mathematical weights/rules of the model.
2. **Samples:** The actual data you are processing (e.g., images, text).
If you process one sample at a time, the GPU has to load the model parameters into the fast cache, load the single sample, do the math, and then flush it out to do the next one.

By grouping samples into a **batch** (e.g., 32 or 64 samples at once), the GPU loads the heavy model parameters into the ultra-fast cache **only once**. It then applies those same parameters to all 32 samples simultaneously. Because the math happens in parallel and you skipped the chore of moving the parameters 31 extra times, processing a whole batch takes roughly the same amount of time as processing a single sample.

Next: [[Tensors]]
