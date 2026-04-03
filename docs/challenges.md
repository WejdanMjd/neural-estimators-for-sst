# ⚠️ Challenges Encountered and Solutions

During the development of this project, several technical and practical challenges arose. Addressing them was essential to building a robust and reproducible pipeline.

---

### 1. Data Format and Consistency

* **Challenge:** The processed datasets were stored in `.jld2` files with specific internal keys (e.g., `Z_train_processed`, `theta_train`). Ensuring consistency between stored variable names and loading logic was critical.
* **Solution:** We carefully inspected dataset keys and aligned the loading step using:
    ```julia
    @load train_path Z_train_processed theta_train
    ```
    This ensured correct mapping between stored data and variables used in training.

---

### 2. Variable-Length Temporal Dimension

* **Challenge:** The original spatio-temporal data had variable sequence lengths, which are incompatible with standard neural network batching.
* **Solution:** We applied first-order temporal differencing and standardized all samples to a fixed temporal length ($T = 9$).
    This made the data:
    * **Consistent** in shape.
    * **Compatible** with batching.
    * **Suitable** for DeepSet processing.

---

### 3. Float64 vs Float32 Mismatch (Performance Warning)

* **Challenge:** Flux raised warnings due to mismatch between model parameters (`Float32`) and input data (`Float64`), causing slower computation.
* **Solution:** We converted all inputs to `Float32` during preprocessing:
    ```julia
    Z_train_processed = [Float32.(z) for z in Z_train_processed]
    ```
    This removed warnings and improved performance, especially on GPU.

---

### 4. GPU Not Being Detected Properly

* **Challenge:** Despite having CUDA installed, the model initially ran on CPU and produced warnings such as *"No functional GPU backend found"*.
* **Solution:** We properly installed/built CUDA dependencies (`CUDA.jl`, `cuDNN`), activated the correct environment, and restarted the kernel. Verified using:
    ```julia
    CUDA.functional()
    ```
    Then explicitly moved the model:
    ```julia
    network = gpu(network)
    ```

---

### 5. Model Not Moving to GPU

* **Challenge:** Even after enabling CUDA, the model remained on CPU due to incorrect parameter handling.
* **Solution:** We ensured the model was moved using `gpu(network)` and the estimator was re-initialized afterward:
    ```julia
    NBE = PointEstimator(network)
    ```

---

### 6. API Misuse (PointEstimator Initialization)

* **Challenge:** An incorrect attempt was made to pass extra arguments (`num_summaries`) to `PointEstimator`, leading to a method error.
* **Solution:** We corrected the initialization to match the official API:
    ```julia
    NBE = PointEstimator(network)
    ```

---

### 7. Git Push Rejection (Remote Conflict)

* **Challenge:** Git rejected the push because the remote repository was ahead: `! [rejected] main -> main (fetch first)`.
* **Solution:** We resolved it using a rebase to preserve a clean history:
    ```bash
    git pull --rebase origin main
    git push origin main
    ```

---

### 8. Visualization Not Displaying in Notebook

* **Challenge:** Plots generated using `AlgebraOfGraphics.plot()` did not display in the notebook.
* **Solution:** We explicitly rendered the plot using:
    ```julia
    display(fig)
    ```

---

### 9. Saving Plots Correctly

* **Challenge:** Plots were initially not saved because they were not assigned to a variable.
* **Solution:** We stored the plot object and then saved it:
    ```julia
    fig = AlgebraOfGraphics.plot(assessment)
    save("../assets/assessment_results.png", fig)
    ```