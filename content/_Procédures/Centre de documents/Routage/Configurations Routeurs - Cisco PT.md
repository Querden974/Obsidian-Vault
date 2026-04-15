

**R4**
```
enable
config t
router rip
version 2
no auto-summary
network 23.0.0.4
network 23.0.0.0
```
**R5**
```
enable
config t
router rip
version 2
no auto-summary
network 23.0.0.12
network 23.0.0.0
```
**R6**
```
enable
config t
router rip
version 2
no auto-summary
network 23.0.0.4
network 23.0.0.8
```
**R7**
```
enable
config t
router rip
version 2
no auto-summary
network 23.0.0.8
network 23.0.0.12
```