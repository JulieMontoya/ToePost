# SCREENSHOTS

## A working "4032"  (actually an upgraded 4016)

_These pictures were kindly supplied by Colin._

### The workspace test
![PXL_20250610_181108409.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_181108409.jpg) "The workspace test"

### After the stack and refresh tests
![PXL_20250610_181157051.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_181157051.jpg) "After the stack and refresh tests"
At the end of the stack test, it will contain all zeros.  If, during
the pause after the stack test while we have not been making enough
memory accesses to keep the memory refreshed, any bits have changed
from 0 to 1, this will be picked up as a fault with the refresh
circuit.


### After the zero page test
![PXL_20250610_181238057.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_181238057.jpg) "After the zero page test"
By this point, we can be reasonably confident that we have a
mostly-working machine.

### Testing for address clashes
![PXL_20250610_181313294.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_181313294.jpg) "Testing for address clashes"
Pages 02, 04, 08, 10, 20 and 40 are tested in turn for address clashes
with page 00.  Such a fault would indicate a problem with one of the
high-order address lines A8-A14.  Unpopulated memory locations read
back as the high-order byte of their addresses, so this test will
indicate how much memory is fitted.

### Page-by-page memory testing
![PXL_20250610_182021618.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_182021618.jpg) "Page-by-page memory testing"
Each page from 02 to 7F is tested in turn for readback

### Testing the upper 16K
![PXL_20250610_185304693.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_185304693.jpg) "Testing the upper 16K"

### Finished!
![PXL_20250610_192234876.jpg](https://github.com/JulieMontoya/ToePost/blob/main/screenshots/PXL_20250610_192234876.jpg) "Finished!"
The PET has had its screen memory and program memory tested for stuck
zeros, stuck ones, address clashes and refresh faults, and passed all
the tests.

