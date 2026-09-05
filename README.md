# Enzyme-Kinetics
Copy and Pastable code for Kinetic modeling in MatLab 
Guide:
1st order
%% First-Order vs Full Michaelis-Menten Kinetics Simulation
Copy Starting from line 8

% --- Kinetic Parameters ---
Vmax = ;    % Maximum velocity (uM/min)
Km   = ;    % Michaelis constant (uM)
S0   = ;    % Initial substrate concentration (uM) -> Ensure S0 << Km for 1st order behavior

% --- Time Vector ---
tspan = [ ]; % Time range [start_time end_time] 

% --- First-Order Rate Constant ---
% At [S] << Km, k_first_order = Vmax / Km
k1 = Vmax / Km; 

% --- ODE Differential Equations ---
% Full Michaelis-Menten: dS/dt = - (Vmax * S) / (Km + S)
ode_full = @(t, S) -(Vmax * S) / (Km + S);

% First-Order Approximation: dS/dt = - k1 * S
ode_first_order = @(t, S) -k1 * S;

% --- Solve System using ode45 ---
[t_full, S_full]   = ode45(ode_full, tspan, S0);
[t_first, S_first] = ode45(ode_first_order, tspan, S0);

% Analytical solution for 1st order: S(t) = S0 * exp(-k1 * t)
S_analytical = S0 * exp(-k1 * t_first);

% --- Plotting Results ---
figure('Color', 'w', 'Position', [100 100 800 500]);

plot(t_full, S_full, 'b-', 'LineWidth', 2, 'DisplayName', 'Full Michaelis-Menten');
hold on;
plot(t_first, S_first, 'r--', 'LineWidth', 2, 'DisplayName', '1st-Order Numerical (ode45)');
plot(t_first, S_analytical, 'ko', 'MarkerSize', 4, 'DisplayName', '1st-Order Analytical [S_0 e^{-kt}]');

grid on;
xlabel('Time (min)', 'FontSize', 12);
ylabel('Substrate Concentration [S] (\muM)', 'FontSize', 12);
title(['Enzyme Kinetics at Low Substrate Concentration ([S_0] = ' num2str(S0) ' \muM, K_m = ' num2str(Km) ' \muM)'], 'FontSize', 13);
legend('Location', 'northeast', 'FontSize', 10);

% --- Display Calculated Rate Constant in Command Window ---
fprintf('===========================================\n');
fprintf('First-Order Rate Constant (k) = %.4f min^-1\n', k1);
fprintf('Half-life (t_1/2)            = %.4f min\n', log(2)/k1);
fprintf('===========================================\n');
