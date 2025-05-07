# AIR QUALITY INDEX PREDICTION AI MODEL
AI-Internship


% Clear workspace
clc; clear; close all;
 
%% Parameters
num_UE = 20;                     % Number of UEs
RAN_bandwidth = 100e6;           % 100 MHz RAN bandwidth
data_rate = 1e9;                  % 1 Gbps throughput
latency = 5;                      % 5 ms latency
 
% Simulation time in seconds
sim_time = 10;                    
time = linspace(0, sim_time, sim_time);
 
% Initializing Variables
data_usage = randi([10, 200], num_UE, sim_time); % Random data usage (MB)
latency_variation = latency + randi([-2, 2], num_UE, sim_time); % Latency fluctuation
 
%% Simulating 5G RAN and Core Interaction
disp('Initializing 5G RAN and Core Network Simulation...');
 
% Simulate Data Transmission
data_transmitted = zeros(1, sim_time);
bandwidth_utilization = zeros(1, sim_time);
 
for t = 1:sim_time
    total_data = sum(data_usage(:, t)) * 1e6; % Convert MB to bytes
    data_transmitted(t) = total_data / 1e9;   % Convert to GB
    bandwidth_utilization(t) = (total_data / (RAN_bandwidth)) * 100;  % Utilization in %
end
 
% Display Results
disp('Simulation Completed!');
fprintf('Total Data Transmitted: %.2f GB\n', sum(data_transmitted));
fprintf('Average Latency: %.2f ms\n', mean(mean(latency_variation)));
 
%% Plotting Results
figure;
 
% Plot 1: Data Transmitted over Time
subplot(2, 2, 1);
plot(time, data_transmitted, 'b-o', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Data Transmitted (GB)');
title('Data Transmission over Time');
grid on;
 
% Plot 2: Bandwidth Utilization
subplot(2, 2, 2);
plot(time, bandwidth_utilization, 'r--', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Bandwidth Utilization (%)');
title('Bandwidth Utilization over Time');
grid on;
 
% Plot 3: Latency per UE
subplot(2, 2, 3);
plot(time, mean(latency_variation, 1), 'g-', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Average Latency (ms)');
title('Latency Variation over Time');
grid on;
 
% Plot 4: Data Usage per UE
subplot(2, 2, 4);
plot(time, sum(data_usage, 1), 'm-', 'LineWidth', 2);
xlabel('Time (s)');
ylabel('Total Data Usage (MB)');
title('Total Data Usage by UEs');
grid on;
 
%% Performance Summary
figure;
bar(1:num_UE, mean(data_usage, 2));
xlabel('UE ID');
ylabel('Average Data Usage (MB)');
title('Average Data Usage per UE');
grid on;

