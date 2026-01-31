Retinal Vessel Extraction via Hessian-Based Curvature Analysis

1. Project Overview
This project implements a research-level computer vision pipeline to extract blood vessels from retinal images (e.g., the DRIVE dataset).
Unlike simple edge detection, this method uses Second-Order Calculus (Hessian Matrices) to identify tubular structures based on their geometric "curvature" rather than just their brightness.


2. Step-by-Step Implementation
   Step 1: Gaussian Smoothing (Signal Stability)Why? Differentiation is a high-pass filter that amplifies noise. We convolve the image with a Gaussian kernel ($G_\sigma$). This ensures the Taylor expansion is "well-behaved" and stable, fulfilling the requirement for reasoning about optimization stability.
   Step 2: Computing the HessianWe calculate the partial derivatives across the entire image.If a vessel is horizontal, the "bend" across its width ($I_{yy}$) will be large, while the "bend" along its length ($I_{xx}$) will be near zero.
   Step 3: Eigendecomposition (Geometric Rotation)The raw Hessian values depend on the orientation of the vessel. To make our detector Rotationally Invariant, we calculate the Eigenvalues ($\lambda_1, \lambda_2$).
           Eigenvectors: Point in the direction of the "steepest" and "flattest" curvature.Eigenvalues: Provide a numerical "score" for that curvature.
   Step 4: Vesselness Scoring (Frangi Filter Logic) We apply a filter that distinguishes between three types of structures:
           Vessels (Tubular): One eigenvalue λ1 is huge , one is near zero.
           Blobs (Isotropic): Both eigenvalues are large.
           Background (Flat): Both eigenvalues are near zero.

3. Concept used in this project
   1. Coordinate Invariance: By using Eigenvalues, we ensure the algorithm detects a vessel whether it is horizontal, vertical, or diagonal.
      This is a leap from 1D signal processing to 2D manifold analysis.
   2. Multivariable Optimization: We use the second-order properties of the image manifold to suppress noise that a first-order derivative (like a Sobel filter) would incorrectly label as a vessel.
   3. Scale-Space Analysis: The choice of $\sigma$ in our Gaussian smoothing allows us to target vessels of specific diameters, demonstrating an understanding of frequency-domain behavior.
  
4. How to Run
Clone the repository.

Ensure you have numpy, matplotlib, and scikit-image installed.

Run the Python script to see the transformation from raw retinal signals to geometrically extracted vascular maps.
 
