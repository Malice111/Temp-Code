#include <SevSeg.h>
SevSeg sevseg;

/* -- TIMER -- */
static int seconds = 0;

/* -- Buzz Game -- */
static int touchCount = 0;
const int touchPin = A0;
const int touchThreshold = 1000;
const int maxTouchCount = 20;
const unsigned long touchDelay = 500;
const unsigned long resetDelay = 2000;

/* -- 7-Segment -- */
const byte numDigits = 4;
const byte digitPins[] = {2, 5, 6, 8};
const byte segmentPins[] = {3, 7, 12, 10, 9, 4, 13, 11};
const bool resOnSeg = false;

void setup() { 
  // Start the 7-Segment instane
  sevseg.begin(COMMON_CATHODE, numDigits, digitPins, segmentPins, resOnSeg);
  // Set screen brightness to 100, range is (-200 || +200)
  sevseg.setBrightness(100);
}

void loop() {
  // Read sensor value
  int touchValue = analogRead(touchPin);
  static bool wasTouched = false;

  // Check if touch value exceeds threshold
  if (touchValue >= touchThreshold) {
    // Check if it wasn't touched previously
    if (!wasTouched) {
      // Increment touch count
      touchCount++;
      // Check if touch count exceeds maximum
      if (touchCount >= maxTouchCount) {
        touchCount = 0; // Reset touch count
        failProc(); // call failProc function
      } else {
        // Display current touch count
        sevseg.setNumber(touchCount, 0);
        delay(touchDelay);
      }
    }
    wasTouched = true; // Set wasTouched to true
  } else {
    wasTouched = false; // Set wasTouched to false
  }

  // If touch count is 0, display 0
  if (touchCount == 0) {
    sevseg.setNumber(0, 0);
  }

  // Start counting (from Arduino startup)
  static unsigned long timer = millis();
  
  // Increment timer every second
  if (millis() - timer >= 1000) {
    timer += 1000;
    seconds++;
    
    if (seconds == 1000) {
      seconds = 0; // Reset seconds
    }
    // Extract last two digits of the timer
    int lastTwoDigits = seconds % 100;

    // Constrain the timer, hence display the value
    sevseg.setNumber(lastTwoDigits, 2);
  }

  // Check if the timer exceeds 60 seconds without exceeding maxTouchCount
  if (seconds >= 60 && touchCount < maxTouchCount) {
    winProc(); // Call winProc function
  }

  // Refresh the display
  sevseg.refreshDisplay();
}

auto failProc() -> void {
  sevseg.blank(); // Clear the display
  // Blink the fail message thrice
  for (int i = 0; i < 3; ++i) {
    sevseg.setChars("FAIL");
    delay(300); // Display "FAIL" for 300 milli
    sevseg.blank(); // Clear display
    delay(300); // Delay for 300 milli
  }
}
auto winProc() -> void {
  sevseg.blank(); // Clear the display
  // Blink the win message thrice
  for (int i = 0; i < 3; ++i) {
    sevseg.setChars("000");
    delay(300); // Display "WIN" for 300 milli
    sevseg.blank(); // Clear display
    delay(300); // Delay for 300 milli
  }
}
