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
  <i>Power Spectral Density (PSD)</i> of a discrete-time signal. Here a window function is applied to the finite-length signal before computing its Fourier transform. The use of a window significantly reduces spectral leakage, which arises from the abrupt truncation of finite-duration signals, thereby providing a more reliable estimate of the signal spectrum. It enhances the classical periodogram by mitigating spectral leakage through the application of a windowing function. 
</p>

<h3>Classical Periodogram</h3>

<p>
  The periodogram is a non-parametric PSD estimation method based on the Discrete Fourier Transform (DFT):
</p>

<p>
  \[
  P_x(f) = \frac{1}{N} \left| X(f) \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(X(f)\) : DFT of the signal \(x[n]\)</li>
  <li>\(N\) : Signal length</li>
</ul>

<p>
  However, the classical periodogram suffers from <b>spectral leakage</b> due to abrupt truncation of the signal.
</p>

<h3>Windowing to Mitigate Spectral Leakage</h3>

<p>
  Spectral leakage can be minimized by applying a <b>window function</b> \(w[n]\) 
  to the signal before computing the DFT. The resulting PSD estimate is called the 
  <i>windowed periodogram</i>:
</p>

<p>
  \[
  P_w(f) = \frac{1}{N \cdot U} \left| X_w(f) \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(w[n]\) : Window function</li>
  <li>\(X_w(f)\) : DFT of the windowed signal \(x[n] \cdot w[n]\)</li>
  <li>\(U = \frac{1}{N} \sum_{n=0}^{N-1} |w[n]|^2\) : Normalization factor</li>
</ul>
<p>
Windowing smooths the discontinuities at the signal boundaries, thereby reducing sidelobes in the spectral estimate. However, this improvement comes at the expense of slightly reduced frequency resolution due to the widening of the main lobe.
  </p>
<h3>Common Window Functions</h3>

<ul>
  <li><b>Rectangular Window:</b> Equivalent to the classical periodogram  
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

  <li><b>Hanning Window:</b> Provides smoother transitions  
  \[
  w[n] = 0.5 \left(1 - \cos\left(\frac{2 \pi n}{N-1}\right)\right), \quad 0 \le n \le N-1
  \]</li>
</ul>

<h3>Implementation Steps</h3>
<ol>
  <li><b>Segment the Signal:</b> Divide the signal into overlapping or non-overlapping segments of length \(N\).</li>
  <li><b>Apply a Window Function:</b> Multiply each segment by a window function \(w[n]\).</li>
  <li><b>Compute the DFT:</b> Calculate the DFT of the windowed segments.</li>
  <li><b>Average the Periodograms:</b> For overlapping segments, average the periodograms to reduce variance.</li>
</ol>

<h3>Applications</h3>
<ul>
  <li>Signal Processing: Analyzing frequency content of time-varying signals.</li>
  <li>Communications: Evaluating spectrum occupancy in wireless systems.</li>
  <li>Bioinformatics: Investigating periodicities in biological signals (e.g., EEG, ECG).</li>
  <li>Seismology: Characterizing seismic wave frequencies.</li>
</ul>


<h2>Correlogram</h2>

<p>
  The <b>Correlogram</b> method estimates the PSD based on the Fourier Transform of the estimated autocorrelation function. Unlike the periodogram, which estimates the spectrum directly from the signal, the correlogram estimates the PSD by computing the Fourier transform of the signal's autocorrelation function. This approach is based on the Wiener–Khinchin theorem, which states that the PSD is the Fourier transform of the autocorrelation sequence.
  
</p>

<h3>PSD via Autocorrelation</h3>

<p>
  \[
  S_x(f) = \sum_{k=-N+1}^{N-1} R_x[k] e^{-j 2 \pi f k}
  \]
</p>

<p>Where \(R_x[k]\) is the autocorrelation function of the discrete-time signal.</p>

<h3>Autocorrelation Function</h3>

<p>
  For a discrete-time signal \(x[n]\):
</p>

<p>
  \[
  R_x[k] = \frac{1}{N} \sum_{n=0}^{N-1-k} x[n] x^*[n+k]
  \]
</p>

<p>Where \(k\) is the lag.</p>

<h3>Implementation Steps</h3>
<ol>
  <li>Estimate autocorrelation.</li>
  <li>Apply a window to the autocorrelation sequence.</li>
  <li>Compute the Fourier Transform to estimate the PSD.</li>
</ol>

<h3>Windowing in Correlogram</h3>
<ul>
  <li><b>Rectangular Window:</b> No additional processing.</li>
  <li><b>Hamming Window:</b> Reduces sidelobes.</li>
  <li><b>Hanning Window:</b> Smooth transitions at edges.</li>
</ul>

<h3>Advantages</h3>
<ul>
  <li>Simple to implement.</li>
  <li>Provides insight into the frequency-domain characteristics of signals.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Limited frequency resolution due to finite data length.</li>
  <li>Potential for spectral leakage if no windowing is applied.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Analyzing stationary signals in time-series data.</li>
  <li>Frequency-domain analysis in communication systems.</li>
  <li>Studying periodic patterns in biological signals.</li>
</ul>


<h2>Bartlett Method</h2>

<p>
 The Bartlett method is an extension of the classical periodogram that aims to reduce the large variance associated with a single periodogram estimate. Instead of estimating the PSD from the entire data record, the available signal is divided into several non-overlapping segments of equal length. A separate periodogram is computed for each segment, and the individual estimates are then averaged to obtain the final PSD.
Averaging multiple periodograms reduces random fluctuations in the spectral estimate, resulting in a smoother and more reliable PSD. However, because each segment contains fewer samples than the original signal, the method sacrifices some frequency resolution in exchange for lower variance. Thus, the Bartlett method provides an effective balance between estimation stability and spectral resolution.

</p>

<p>
  \[
  P_x(f_k) = \frac{1}{M} \sum_{m=0}^{M-1} |X_m[k]|^2
  \]
</p>

<p>Where \(X_m[k]\) is the DFT of the m-th segment, \(M\) is the number of segments.</p>

<h3>Implementation Steps</h3>
<ol>
  <li>Segment the signal into M non-overlapping parts.</li>
  <li>For each segment, calculate the periodogram.</li>
  <li>Average the periodograms of all segments.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces variance in PSD estimation compared to a single periodogram.</li>
  <li>Simple to implement.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Loss of frequency resolution due to segmenting the signal.</li>
  <li>Bias may remain if the signal is not stationary within segments.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Analyzing frequency content in stationary signals.</li>
  <li>Estimating PSD in communication systems.</li>
</ul>


<h2>Blackman-Tukey Method</h2>

<p>
  The Blackman–Tukey method combines the concepts of autocorrelation-based spectrum estimation and windowing. Instead of directly computing the Fourier transform of the estimated autocorrelation sequence, a suitable window is first applied to the autocorrelation values before performing the transform. This window suppresses the contribution of unreliable autocorrelation estimates at large lags, thereby reducing estimation variance and spectral fluctuations.
By selecting an appropriate window function, the Blackman–Tukey method offers flexibility in controlling the trade-off between frequency resolution and spectral smoothness. Consequently, it provides more stable PSD estimates than the basic correlogram method, particularly when only a limited amount of data is available.

</p>

<p>
  \[
  P_x(f) = \text{FFT}\{ R_x[k] \cdot w[k] \}
  \]
</p>

<p>Where \(w[k]\) is the window function applied to autocorrelation \(R_x[k]\).</p>

<h3>Implementation Steps</h3>
<ol>
  <li>Compute the Autocorrelation.</li>
  <li>Apply a Window Function.</li>
  <li>Perform Fourier Transform.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces spectral leakage through windowing.</li>
  <li>Smoothens the power spectral estimate, reducing variance.</li>
  <li>Flexibility in choosing different window functions (e.g., Hamming, Hanning, Blackman).</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Lower frequency resolution due to windowing effects.</li>
  <li>Computationally expensive for large signals.</li>
  <li>Accuracy depends on the choice of window length and type.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Analysis of radar and sonar signals.</li>
  <li>Audio and speech signal processing.</li>
  <li>Power spectral density estimation in communications systems.</li>
</ul>


<h2>Welch Method</h2>

<p>
 The Welch method is one of the most widely used non-parametric techniques for PSD estimation because it significantly improves upon both the classical periodogram and the Bartlett method. Similar to Bartlett's approach, the signal is divided into several shorter segments. However, unlike Bartlett's method, the segments are allowed to overlap, and each segment is multiplied by a window function before computing its periodogram.
The overlap increases the number of available spectral estimates without requiring additional data, while the window function reduces spectral leakage. Averaging these windowed periodograms produces a PSD estimate with considerably lower variance and improved statistical stability. Due to its excellent compromise between frequency resolution, variance reduction, and computational efficiency, Welch's method has become one of the standard techniques for practical power spectral density estimation.

</p>

<p>
  \[
  P_{xx}(f) = \frac{1}{K} \sum_{k=0}^{K-1} \left| \text{FFT}\{ x_k[n] \cdot w[n] \} \right|^2
  \]
</p>

<p>Where:</p>
<ul>
  <li>\(K\) : Number of segments</li>
  <li>\(L\) : Length of each segment</li>
  <li>\(w[n]\) : Window function</li>
  <li>\(x_k[n]\) : kth segment of the signal</li>
</ul>

<h3>Implementation Steps</h3>
<ol>
  <li>Divide the signal into overlapping segments (typically 50% overlap).</li>
  <li>Apply a window (e.g., Hamming or Blackman) to each segment to reduce spectral leakage.</li>
  <li>For each segment, compute the periodogram using the Fast Fourier Transform (FFT).</li>
  <li>Average the periodograms of all segments to obtain the final PSD estimate.</li>
</ol>

<h3>Advantages</h3>
<ul>
  <li>Reduces variance by averaging multiple segments.</li>
  <li>Allows flexibility in segment length, overlap, and window choice.</li>
  <li>Minimizes spectral leakage with windowing.</li>
</ul>

<h3>Limitations</h3>
<ul>
  <li>Lower frequency resolution due to overlap and windowing.</li>
  <li>Increased computational cost for larger signals.</li>
</ul>

<h3>Applications</h3>
<ul>
  <li>Communications & Wireless Systems</li>
  <li>Biomedical Signals (EEG, ECG, EMG)</li>
  <li>Audio & Speech Processing</li>
  <li>Mechanical & Vibration Analysis</li>
  <li>Radar & Sonar</li>
</ul>

</body>
</html>
