% Car Parameters
% Formula 1 Car Standard
engine_power = 120 * 1000; % Convert kW to watts
car_mass = 798 + 80; % kg (car + driver)
drag_co = 1; % 0.7 to 1.4 for F1 cars
A = 1.3; % Frontal area m^2

% F1 Track Layout: A loop of straights and curves
track = [
    struct('type', 'straight', 'length', 300, 'radius', NaN),
    struct('type', 'curve', 'length', 150, 'radius', 30),
    struct('type', 'straight', 'length', 400, 'radius', NaN),
    struct('type', 'curve', 'length', 100, 'radius', 20),
    struct('type', 'straight', 'length', 200, 'radius', NaN),
    struct('type', 'curve', 'length', 250, 'radius', 50),
    struct('type', 'straight', 'length', 600, 'radius', NaN),
    struct('type', 'curve', 'length', 200, 'radius', 40)
];

% Function to calculate time and final speed on straights and curves
function [time, v_final] = calculate_segment_time(section, v_initial, car_mass, engine_power)
    if strcmp(section.type, 'straight')
        force = engine_power / max(v_initial, 1); % Avoid division by zero
        acceleration = force / car_mass; % Newton's second law
        v_final = sqrt(v_initial^2 + 2 * acceleration * section.length);
        v_avg = (v_initial + v_final) / 2;
        time = section.length / v_avg;
    elseif strcmp(section.type, 'curve')
        v_final = sqrt(9.81 * section.radius); % Centripetal force limit
        v_avg = min(v_initial, v_final); % Reduce speed in curves
        time = section.length / v_avg;
    end
end

% Simulate multiple laps
num_laps = 3; % Set number of laps
total_time = 0;
v_initial = 0; % Initial speed
distances = [0]; % Start with distance 0
speeds = [v_initial]; % Start with initial speed

for lap = 1:num_laps
    for i = 1:length(track)
        section = track(i);
        [time, v_final] = calculate_segment_time(section, v_initial, car_mass, engine_power);
        
        % Accumulate the lap time and store distances and speeds
        total_time = total_time + time;
        distances(end+1) = distances(end) + section.length;
        speeds(end+1) = v_final;
        
        % Update the initial speed for the next section
        v_initial = v_final;
    end
end

disp(total_time)

% Plot the speed profile along the track
figure;
plot(distances, speeds, '-o');
xlabel('Distance (m)');
ylabel('Speed (m/s)');
title(['Speed Profile along the Track - ' num2str(num_laps) ' Lap(s)']);
