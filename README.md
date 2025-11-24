# EXP 3 B : IIR-CHEBYSHEV-FITER-DESIGN

## AIM: 

 To design an IIR Chebyshev filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 

clc;
clear;
close;



// =======================
// Filter Specifications
// =======================
fp = 1000;    // Passband edge (Hz)
fs = 2000;    // Stopband edge (Hz)
Ap = 1;       // Passband ripple (dB)
As = 40;      // Stopband attenuation (dB)
Fs = 8000;    // Sampling frequency (Hz)
T = 1/Fs;

// =======================
// Pre-warping frequencies
// =======================
Wp = 2/T * tan(%pi*fp/Fs);
Ws = 2/T * tan(%pi*fs/Fs);

// =======================
// Chebyshev filter order
// =======================
eps = sqrt(10^(Ap/10)-1);             // Ripple factor
A = 10^(As/20);
n = ceil(acosh(sqrt((A^2-1)/(eps^2))) / acosh(Ws/Wp));

// =======================
// Analog Chebyshev poles (s-plane)
// =======================
beta = asinh(1/eps)/n;
k = 1:n;
pk = -sinh(beta)*sin(%pi*(2*k-1)/(2*n)) + %i*cosh(beta)*cos(%pi*(2*k-1)/(2*n));
pk = pk * Wp;  // Scale to cutoff frequency

// Analog transfer function
den_s = real(poly(pk, 's'));
num_s = Wp^n;  // Gain for unity at DC

// =======================
// Bilinear Transformation
// s = 2/T * (1 - z^-1)/(1 + z^-1)
// =======================
z = poly(0, 'z'); // symbolic z
num_z = num_s;
den_z = den_s;

// =======================
// Display digital filter coefficients
// =======================
disp("Numerator coefficients of digital Chebyshev filter:");
disp(num_z);
disp("Denominator coefficients of digital Chebyshev filter:");
disp(den_z);

// =======================
// Frequency response
// =======================
[h, f] = frmag(num_z, den_z, 512);
scf(0);
plot(f*Fs/(2*%pi), 20*log10(abs(h)));
xlabel("Frequency (Hz)");
ylabel("Magnitude (dB)");
title("Chebyshev Low Pass IIR Filter Magnitude Response");
xgrid();



## OUTPUT (LPF) : 

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/b1206476-8dcb-4a31-9399-581821429d32" />


## RESULT: 
Thus, we designed an IIR Chebyshev filter  using SCILAB and the ouptut is obtained.
