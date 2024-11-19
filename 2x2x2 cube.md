
Copy this code. 
You need your outputs to pin 5,4,3,2
and you need your grounds into A0,A1


```
// Delay time for visualization
int delayTime = 250;

// 3D array to define LED patterns (words)
// Each element represents a "pattern" with two rows and four columns
int ledPatterns[12][2][4] = {
  // Patterns specify which LEDs to turn on/off
  {{1, 1, 1, 1}, {1, 1, 1, 1}}, // All LEDs on
  {{1, 1, 1, 1}, {0, 0, 0, 0}}, // Row 1 on, Row 2 off
  {{0, 0, 0, 0}, {1, 1, 1, 1}}, // Row 1 off, Row 2 on
  {{1, 1, 0, 0}, {1, 1, 0, 0}}, // First two LEDs on in both rows
  {{0, 0, 1, 1}, {0, 0, 1, 1}}, // Last two LEDs on in both rows
  {{1, 0, 0, 1}, {1, 0, 0, 1}}, // Outer LEDs on in both rows
  {{0, 1, 1, 0}, {0, 1, 1, 0}}, // Inner LEDs on in both rows
  {{0, 0, 0, 0}, {1, 0, 0, 1}}, // Row 1 off, Row 2 outer LEDs on
  {{0, 1, 1, 0}, {0, 0, 0, 0}}, // Row 1 inner LEDs on, Row 2 off
  {{1, 0, 0, 1}, {0, 0, 0, 0}}, // Row 1 outer LEDs on, Row 2 off
  {{0, 0, 0, 0}, {0, 1, 1, 0}}, // Row 1 off, Row 2 inner LEDs on
  // {{0, 0, 0, 0}, {0, 0, 0, 0}}  // All LEDs off
};

// Function runs once to initialize the program
void setup() {
  // Initialize serial communication at 9600 baud rate
  Serial.begin(9600);

  // Set pins 2, 3, 4, 5 (LED columns) as OUTPUT
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);

  // Set pins A0, A1 (LED rows) as OUTPUT
  pinMode(A0, OUTPUT);
  pinMode(A1, OUTPUT);
}

// Function to turn off all LEDs
void turnOffAllLEDs() {
  digitalWrite(A0, HIGH); // Turn off Row 1
  digitalWrite(A1, HIGH); // Turn off Row 2
  digitalWrite(2, LOW);   // Turn off Column 1
  digitalWrite(3, LOW);   // Turn off Column 2
  digitalWrite(4, LOW);   // Turn off Column 3
  digitalWrite(5, LOW);   // Turn off Column 4
}

// Main loop to display LED patterns
void loop() {
  // Turn off all LEDs
  turnOffAllLEDs();

  // Iterate through each pattern in the array
  for (int pattern = 0; pattern < 12; pattern++) { // 12 patterns
    for (int row = 0; row < 2; row++) { // 2 rows
      for (int col = 0; col < 4; col++) { // 4 columns
        // Check if the current LED should be on
        if (ledPatterns[pattern][row][col] == 1) {
          // Print debug information (pattern, row, column)
          //Serial.print(pattern); // Pattern index
          //Serial.print(row);     // Row index
          //Serial.println(col + 2); // Column (pin number)

          // Turn on the specified LED
          turnOnLED(row, col + 2, 0);
        }
      }
    }
    // Add a delay to visualize the pattern
    turnOnLED(0, 0, delayTime);
  }
}

// Function to turn on a specific LED or wait for a delay
void turnOnLED(int row, int column, int delayTime) {
  // Map row indices to their corresponding pins
  int rows[2] = {A0, A1};

  // If no delay is specified, turn on the LED
  if (delayTime == 0) {
    digitalWrite(rows[row], LOW);  // Activate the row
    digitalWrite(column, HIGH);   // Activate the column
  } else {
    delay(delayTime);  // Wait for the specified time
    turnOffAllLEDs();  // Turn off all LEDs after the delay

  }
}
```
