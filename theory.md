<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
</head>
<body>

<h2>Windowed Periodogram</h2>

<p>
  The <b>windowed periodogram</b> is a widely used technique for estimating the 
  <i>Power Spectral Density (PSD)</i> of a discrete-time signal. It improves the classical periodogram 
  by mitigating spectral leakage through the application of a window function. 
  This is essential for accurate frequency-domain analysis.
</p>

<h3>Classical Periodogram</h3>

<p>
  The periodogram is a non-parametric PSD estimation method based on the Discrete-Time Fourier Transform (DTFT):
</p>

<p>
  \[
  P_x(f) = \frac{1}{N} \left| \sum_{n=0}^{N-1} x[n] e^{-j 2 \pi f n} \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(x[n]\) : Discrete-time signal</li>
  <li>\(N\) : Signal length</li>
</ul>

<p>
  The classical periodogram suffers from <b>spectral leakage</b> due to abrupt truncation of the signal.
</p>

<h3>Windowing to Mitigate Spectral Leakage</h3>

<p>
  Apply a <b>window function</b> \(w[n]\) to the signal before computing the DTFT:
</p>

<p>
  \[
  P_x(f) = \frac{1}{N \cdot U} \left| \sum_{n=0}^{N-1} x[n] w[n] e^{-j 2 \pi f n} \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(w[n]\) : Window function</li>
  <li>\(U = \frac{1}{N} \sum_{n=0}^{N-1} |w[n]|^2\) : Normalization factor to preserve signal power</li>
</ul>

<h3>Common Window Functions</h3>

<ul>
  <li><b>Rectangular Window:</b> Equivalent to no window, sharp edges  
  \[
  w[n] =
  \begin{cases}
  1, & 0 \le n \le N-1 \\
  0, & \text{otherwise}
  \end{cases}
  \]</li>

  <li><b>Hamming Window:</b> Reduces sidelobe amplitudes  
  \[
  w[n] = 0.54 - 0.46 \cos\left(\frac{2 \pi n}{N-1}\right), \quad 0 \le n \le N-1
  \]</li>

  <li><b>Hann Window:</b> Smooth transitions at edges  
  \[
  w[n] = 0.5 \left(1 - \cos\left(\frac{2 \pi n}{N-1}\right)\right), \quad 0 \le n \le N-1
  \]</li>

<li><b>Blackman Window:</b> Further reduces sidelobes at the cost of main-lobe width  
\[
w[n] = 0.42 - 0.5 \cos\left(\frac{2 \pi n}{N-1}\right) + 0.08 \cos\left(\frac{4 \pi n}{N-1}\right), \quad 0 \le n \le N-1
\]
</li>
</ul>

<h3>Implementation Steps</h3>
<ol>
  <li>Segment the signal into overlapping or non-overlapping segments of length \(N\).</li>
  <li>Multiply each segment by a window function \(w[n]\).</li>
  <li>Compute the DTFT or FFT of the windowed segments.</li>
  <li>Average the periodograms to reduce variance.</li>
</ol>

<h3>Applications</h3>
<ul>
  <li>Signal Processing: Analyzing frequency content of time-varying signals.</li>
  <li>Communications: Evaluating spectrum occupancy in wireless systems.</li>
  <li>Biomedical Signal Analysis: Investigating periodicities in physiological signals (EEG, ECG).</li>
  <li>Seismology: Characterizing seismic wave frequencies.</li>
</ul>

<h2>Correlogram Method</h2>

<p>
  Estimates PSD from the DTFT of the estimated autocorrelation function.
</p>

<h3>PSD via Autocorrelation</h3>

<p>
  \[
  P_x(f) = \sum_{k=-(N-1)}^{N-1} R_x[k] \, e^{-j 2 \pi f k}
  \]
</p>

<p>Where \(R_x[k]\) is the autocorrelation function of \(x[n]\) and \(k\) is the lag. In practice, FFT can be used to compute discrete frequency samples.</p>

<h3>Autocorrelation Function</h3>

<p>
  For a discrete-time signal \(x[n]\), the biased estimate of autocorrelation is:
</p>

<p>
  \[
  R_x[k] =
  \begin{cases}
    \frac{1}{N} \sum_{n=0}^{N-1-k} x[n] \, x^*[n+k], & k \ge 0 \\
    R_x^*[-k], & k < 0
  \end{cases}
  \]
</p>

<p>Here, \(k\) is the lag, \(N\) is the number of samples, and \(R_x^*[-k]\) ensures symmetry for negative lags.</p>

<p><b>Note on Biased Estimate:</b> Dividing by \(N\) for all lags makes this a <i>biased estimate</i>. It slightly underestimates autocorrelation for large lags but ensures the PSD is always non-negative. An <i>unbiased estimate</i> divides by \(N-k\), correcting the bias at the cost of possibly introducing negative PSD values.</p>

<h3>Implementation Steps</h3>
<ol>
  <li>Estimate autocorrelation.</li>
  <li>Apply a window to the autocorrelation sequence.</li>
  <li>Compute DTFT (or FFT) to estimate PSD.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Simple to implement.</li>
  <li>Provides insight into frequency-domain characteristics of signals.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Limited frequency resolution due to finite data length.</li>
  <li>Potential for spectral leakage without windowing.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Stationary time-series analysis.</li>
  <li>Frequency-domain analysis in communication systems.</li>
  <li>Study periodic patterns in physiological signals.</li>
</ul>

<h2>Bartlett Method</h2>

<p>
  Estimate PSD by segmenting the signal into \(M\) non-overlapping segments, computing periodograms, and averaging:
</p>

<p>
  \[
  P_x(f) = \frac{1}{M \cdot N} \sum_{m=0}^{M-1} \left| \sum_{n=0}^{N-1} x_m[n] e^{-j 2 \pi f n} \right|^2
  \]
</p>

<p>Where \(x_m[n]\) is the m-th segment, \(M\) is number of segments, and \(N\) is the segment length.</p>

<h3>Implementation Steps</h3>
<ol>
  <li>Segment signal into M non-overlapping parts.</li>
  <li>Compute periodogram of each segment.</li>
  <li>Average all periodograms.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces variance vs single periodogram by a factor of \(M\).</li>
  <li>Simple to implement.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Loss of frequency resolution due to shorter segment lengths.</li>
  <li>Bias if signal non-stationary within segments.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Stationary signal frequency analysis.</li>
  <li>Communication system PSD estimation.</li>
</ul>

<h2>Blackman-Tukey Method</h2>

<p>
  The Blackman-Tukey method estimates the PSD by applying a window to the autocorrelation and computing its DTFT:
</p>

<p>
  \[
  P_x(f) = \sum_{k=-K}^{K} R_x[k] \, w[k] \, e^{-j 2 \pi f k}
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(R_x[k]\) is the autocorrelation of the signal for lag \(k\).</li>
  <li>\(w[k]\) is the window applied to the autocorrelation to reduce spectral leakage.</li>
</ul>

<h3>Implementation Steps</h3>
<ol>
  <li>Compute autocorrelation.</li>
  <li>Apply window function to autocorrelation.</li>
  <li>Compute DTFT (or FFT) to estimate PSD.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces spectral leakage.</li>
  <li>Smoothens PSD, reducing variance.</li>
  <li>Flexible choice of windows (Hamming, Hann, Blackman).</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Lower frequency resolution due to windowing.</li>
  <li>Computationally expensive for large signals.</li>
  <li>Accuracy depends on window type and length.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Radar and sonar analysis.</li>
  <li>Audio and speech processing.</li>
  <li>PSD estimation in communication systems.</li>
</ul>

<h2>Welch Method</h2>

<p>
  Improved periodogram by segmenting with overlap, windowing, and averaging:
</p>

<p>
  \[
  P_x(f) = \frac{1}{K \cdot L \cdot U} \sum_{k=0}^{K-1} \left| \sum_{n=0}^{L-1} x_k[n] w[n] e^{-j 2 \pi f n} \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(K\) : Number of segments</li>
  <li>\(L\) : Segment length</li>
  <li>\(U = \frac{1}{L} \sum_{n=0}^{L-1} |w[n]|^2\) : Normalization factor</li>
  <li>\(x_k[n]\) : k-th segment, length \(L\)</li>
  <li>\(w[n]\) : Window applied to each segment</li>
</ul>

<h3>Implementation Steps</h3>
<ol>
  <li>Divide signal into overlapping segments (typically 50%).</li>
  <li>Apply window (Hamming, Hann, etc.) to each segment.</li>
  <li>Compute DTFT (or FFT) of each windowed segment.</li>
  <li>Average all periodograms to obtain final PSD.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces variance significantly by averaging.</li>
  <li>Flexible segment length, overlap, and window choice.</li>
  <li>Minimizes spectral leakage compared to Bartlett.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Lower frequency resolution due to segment length \(L < \text{Total Length}\).</li>
  <li>Higher computational cost for large signals.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Communications and wireless systems.</li>
  <li>Biomedical signals (EEG, ECG, EMG).</li>
  <li>Audio and speech processing.</li>
  <li>Mechanical and vibration analysis.</li>
  <li>Radar and sonar.</li>
</ul>

</body>
</html>