#include <avr/io.h>  // Replace with the appropriate header for your microcontroller

// Function to initialize the ADC
void ADC_Init() {
    // Set the reference voltage to AVCC (5V) and select ADC0
    ADMUX = (1 << REFS0);  // AVCC as reference, ADC0 input
    ADCSRA = (1 << ADEN) | (1 << ADPS2) | (1 << ADPS1);  // Enable ADC, prescaler of 64
}

// Function to read the ADC value from the selected channel
uint16_t ADC_Read() {
    ADCSRA |= (1 << ADSC);  // Start conversion
    while (ADCSRA & (1 << ADSC));  // Wait for conversion to finish
    return ADC;
}

int main() {
    uint16_t adc_value;
    float voltage, actual_voltage;

    ADC_Init();  // Initialize ADC

    while (1) {
        adc_value = ADC_Read();  // Read ADC value (0-1023)

        // Convert ADC value to voltage (0-5V)
        voltage = (adc_value / 1023.0) * 5.0;

        // Convert scaled voltage back to the original input voltage
        actual_voltage = voltage * (200.0 / 10.0);  // Adjust according to your voltage divider

        // Now, actual_voltage holds the measured voltage in the range 0-100V
    }

    return 0;
}
