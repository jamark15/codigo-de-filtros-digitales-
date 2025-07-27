# codigo-de-filtros-digitales-
-señales de entrada-

Fs = 1000;                   % Frecuencia de muestreo (Hz)
t = 0:1/Fs:1-1/Fs;           % Vector de tiempo 1s

% Señal de prueba: suma de senoidales + ruido blanco
f1 = 50;  f2 = 200;  f3 = 400;
x = sin(2*pi*f1*t) + 0.5*sin(2*pi*f2*t) + 0.3*sin(2*pi*f3*t);
x_noisy = x + 0.5*randn(size(t));

% Graficar señal original y ruidosa
figure;
subplot(2,1,1); plot(t, x);      title('Señal limpia');
subplot(2,1,2); plot(t, x_noisy); title('Señal + ruido');


-Filtro PASA BAJOS-
fc_lp = 150/(Fs/2);          % Normalizada
b_fir = fir1(60, fc_lp, 'low', hamming(61));
x_lp_fir = filter(b_fir, 1, x_noisy);

figure;
freqz(b_fir,1,1024,Fs); title('Respuesta en frecuencia FIR pasa bajos');
-filtro-
[b_iir, a_iir] = butter(4, fc_lp, 'low');
x_lp_iir = filter(b_iir, a_iir, x_noisy);

figure;
freqz(b_iir,a_iir,1024,Fs); title('Respuesta en frecuencia IIR pasa bajos');


 Filtro PASA ALTOS
Filtro FIR:

fc_hp = 300/(Fs/2);
b_fir_hp = fir1(60, fc_hp, 'high', hamming(61));
x_hp_fir = filter(b_fir_hp, 1, x_noisy);


Filtro IIR:

[b_iir_hp, a_iir_hp] = butter(4, fc_hp, 'high');
x_hp_iir = filter(b_iir_hp, a_iir_hp, x_noisy);
2.3 Filtro PASA BANDA


Filtro FIR, banda 180-320Hz:


fc_pb = [180 320]/(Fs/2);
b_fir_pb = fir1(60, fc_pb, 'bandpass', hamming(61));
x_pb_fir = filter(b_fir_pb, 1, x_noisy);
Filtro IIR:


[b_iir_pb, a_iir_pb] = butter(4, fc_pb, 'bandpass');
x_pb_iir = filter(b_iir_pb, a_iir_pb, x_noisy);

Visualización de resultados

figure;
subplot(3,1,1); plot(t, x_noisy); title('Señal original + ruido');
subplot(3,1,2); plot(t, x_lp_fir); title('Filtro Pasa Bajos FIR');
subplot(3,1,3); plot(t, x_lp_iir); title('Filtro Pasa Bajos IIR');
