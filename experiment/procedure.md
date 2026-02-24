<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body>
  <ul>
  Choose the spectral estimation method from the numbered Simulation Page options:
        <li><strong>1. Windowed Periodogram:</strong> Click to open a new window to perform periodogram analysis on the input signal.</li>
        <li><strong>2. Correlogram:</strong> Click to open a new window to perform correlogram-based PSD estimation.</li>
        <li><strong>3. Bartlett's Method:</strong> Click to open a new window to perform Bartlett’s PSD estimation.</li>
        <li><strong>4. Blackman-Tukey Method:</strong> Click to open a new window to perform Blackman-Tukey PSD estimation.</li>
        <li><strong>5. Welch's Method:</strong> Click to open a new window to perform Welch’s PSD estimation.</li>
    <li>
      <strong>Input Parameters:</strong>
      <ul>
        <li><strong>Input Signal(s):</strong> Enter the signal frequency (Hz), sampling frequency (Hz), and the signal length. For rectangular or triangular pulse signals, also specify the pulse width in seconds.</li>
        <li><strong>Window Type:</strong> Select a window type (e.g., Rectangular, Hann, Hamming) from the dropdown menu, and provide the window size.</li>
        <li><strong>SNR (in dB):</strong> Enter the desired Signal-to-Noise Ratio (SNR) in decibels (dB).</li>
      </ul>
    </li>
    <br/>
    <h3><strong>Steps:</strong></h3>
    <li>
    <strong>a. For Windowed Periodogram, Correlogram, Bartlett's Method, Blackman-Tukey, & Welch's Method</strong>
    <br/>
      <strong>1. Generate Input Signal(s):</strong> 
      Click the <em>“Generate Input Signal”</em> button to create the input signal. Choose a base signal (e.g., sine, cosine, rectangular, or triangular pulses), and provide the frequency (Hz), sampling frequency (Hz), signal length, window type, window size, and pulse width (for rectangular and triangular pulses) etc.
    </li>
    <li>
      <strong>2. Display the PSD Plot:</strong> 
      Click the <em>“Simulate”</em> button to view the power spectral density (PSD) plot of the noisy input signal.
    </li>
        <strong>b. To Compare all of the above Methods, Choose Option 6 <i>"Compare Different Spectral Estimation Methods"</i> on the Simulation Page </strong>
  </ul>
</body>
</html>

