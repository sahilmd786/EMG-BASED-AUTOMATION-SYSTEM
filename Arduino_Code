const int emgPin = 2; // OUT pin from EMG sensor
const int relayPin = 19; // Relay control pin
int threshold = 1950; // Adjust after calibration
bool relayState = false; // Current relay state
bool lastTrigger = false; // Prevents multiple toggles per flex
unsigned long lastToggleTime = 0; // For debounce
const unsigned long debounceDelay = 500; // 500 ms debounce
void setup() {
Serial.begin(115200); // Use higher baud for smoother plotting
pinMode(relayPin, OUTPUT);
digitalWrite(relayPin, LOW); // Relay OFF initially
}
void loop() {
long sum = 0;
// Take 100 samples
for (int i = 0; i < 100; i++) {
sum += analogRead(emgPin);
delay(5); // Small delay between samples
}
// Compute average
int avgValue = sum / 100;
// --- Plotting section ---
// Print CSV format: EMG average, threshold, relay state
Serial.print(avgValue);
Serial.print(",");
Serial.print(threshold);
Serial.print(",");
Serial.println(relayState ? 1 : 0);
// In Serial Plotter: three lines → EMG signal, threshold line, relay state (0/1)
// --- Relay toggle logic ---
if (avgValue > threshold && !lastTrigger && (millis() - lastToggleTime > debounceDelay)) {
relayState = !relayState; // Flip state
digitalWrite(relayPin, relayState ? HIGH : LOW);
lastTrigger = true; // Mark trigger
lastToggleTime = millis(); // Record time
}
// Reset trigger when muscle relaxes
if (avgValue < threshold) {
lastTrigger = false;
}
}
