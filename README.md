# Frequency-Division-Multiplxing

# AIM

To generate multiple message signals, multiplex them using AM-FDM, and recover each channel using bandpass filtering and coherent demodulation in Scilab.

# APPARATUS REQUIRED

1. Computer with Scilab
2. Scilab script (.sci file)
3. Basic DSP knowledge (filters, convolution, FFT)

# ALGORITHM

1. Generate message signals ( m_i(t) ).
2. Multiply each with its carrier to form AM-DSB.
3. Sum all channels → FDM composite.
4. For each channel:
   a. Band-pass filter around carrier.
   b. Multiply with same carrier (coherent detection).
   c. Low-pass filter to recover baseband.
5. Plot message, FDM, recovered outputs.

# PROCEDURE 

1. Define sampling rate, time vector, message and carrier frequencies.
2. Generate sinusoidal messages and modulate them with cosine carriers.
3. Add all modulated signals to form the composite FDM signal.
4. Design FIR bandpass filter to isolate each channel.
5. Demodulate by mixing with original carrier and low-pass filtering.
6. Plot original messages, FDM signal and recovered signals.
# PROGRM
```scilab
clc; clear; close;
fs=80000; N=floor(0.05*fs); t=(0:N-1)/fs;
fm=[415 425 435 445 455 465];
fc=[4150 4250 4350 4450 4550 4650];
Am=[5.3 5.4 5.5 5.6 5.7 5.8];
Ac=[10.7 10.9 11.1 11.3 11.5 11.7];
num=length(fm);
for i=1:num
    m(i,:)=Am(i)*sin(2*%pi*fm(i)*t);
end
fdm=zeros(1,N);
for i=1:num
    fdm = fdm + Ac(i)*cos(2*%pi*fc(i)*t).*m(i,:);
end
function h=FIR(fc1,fc2,fs,M,mode)
    n=-M:M; L=length(n);
    x1=2*fc1*n/fs; x2=2*fc2*n/fs;
    s1=ones(1,L); s2=ones(1,L);
    for k=1:L
        if x1(k)<>0 then s1(k)=sin(%pi*x1(k))/(%pi*x1(k)); end
        if x2(k)<>0 then s2(k)=sin(%pi*x2(k))/(%pi*x2(k)); end
    end
    lp1=(2*fc1/fs)*s1; lp2=(2*fc2/fs)*s2;
    w=(0.54-0.46*cos(2*%pi*(n+M)/(2*M)));
    if mode==1 then h=lp1.*w;
    else h=(lp2-lp1).*w; end
    h=h/sum(h);
endfunction
M=300;
for i=1:num
    bp=FIR(fc(i)-600,fc(i)+600,fs,M,2);
    iso=conv(fdm,bp,"same");
    mix=iso.*cos(2*%pi*fc(i)*t);
    lp=FIR(800,0,fs,M,1);
    demod(i,:)= (2/Ac(i))*conv(mix,lp,"same");
end
scf(1); clf;
for i=1:num subplot(num,1,i); plot(t,m(i,:)); end
scf(2); clf; plot(t,fdm);
scf(3); clf;
for i=1:num subplot(num,1,i); plot(t,demod(i,:)); end
```
# OUTPUT

<img width="1366" height="616" alt="image" src="https://github.com/user-attachments/assets/c044b2cb-ad73-44ac-b7dc-44d8fa63ec4a" />

<img width="1366" height="616" alt="image" src="https://github.com/user-attachments/assets/a41ea13b-2816-4749-9521-9b83cd7224f9" />

<img width="1366" height="616" alt="image" src="https://github.com/user-attachments/assets/97fafe97-d0ff-4ba8-8f68-ab803c2bb935" />

# MANUAL CALCULATION

<img width="720" height="1280" alt="image" src="https://github.com/user-attachments/assets/8a7fb15e-5626-4499-a798-b62708f669ee" />


# RESULT 

All message frequencies (415–465 Hz) were successfully multiplexed and individually recovered with minimal distortion using bandpass isolation and coherent demodulation.
