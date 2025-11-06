# verilog-HDL-notes


#include <iostream>
#include <cmath>
#include <bitset>
#include <iomanip>
#include <cstdint>
 
// Convert float to fixed-point and print as hex
std::string floatToFixedHex(double value, int total_bits = 32, int int_bits = 10) {
    int frac_bits = total_bits - int_bits;
    int64_t scaling_factor = 1LL << frac_bits;
 
    int64_t fixed_val = static_cast<int64_t>(std::round(value * scaling_factor));
 
    // Handle two's complement for negative values
    if (fixed_val < 0) {
        fixed_val += (1LL << total_bits);
    }
 
    std::stringstream ss;
    ss << "0x" << std::uppercase << std::setfill('0') << std::setw(total_bits / 4)
       << std::hex << (fixed_val & ((1LL << total_bits) - 1));
 
    return ss.str();
}
 
// Convert hex to float (assuming fixed-point)
double fixedHexToFloat(const std::string& hex_str, int total_bits = 32, int int_bits = 10) {
    uint64_t val = std::stoull(hex_str, nullptr, 16);
    int64_t signed_val = static_cast<int64_t>(val);
 
    if (val & (1ULL << (total_bits - 1))) { // If sign bit is set
        signed_val = signed_val - (1LL << total_bits);
    }
 
    int frac_bits = total_bits - int_bits;
    return static_cast<double>(signed_val) / (1LL << frac_bits);
}
 
int main() {
    double input1 = 3.0;
    double input2 = -2.5;
 
    std::string hex1 = floatToFixedHex(input1, 32, 10);
    std::string hex2 = floatToFixedHex(input2, 32, 10);
 
    std::cout << input1 << " in fixed-point hex: " << hex1 << std::endl;
    std::cout << input2 << " in fixed-point hex: " << hex2 << std::endl;
 
    std::cout << hex1 << " back to float: " << fixedHexToFloat(hex1, 32, 10) << std::endl;
    std::cout << hex2 << " back to float: " << fixedHexToFloat(hex2, 32, 10) << std::endl;
 
    return 0;
}
 