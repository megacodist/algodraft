---
status: Planned
---
**Syntax**:

```
CLASS Mapping<KY -> VT> :=
ENDCLASS
```

**Example**:

```
mp AS Mapping<Integer -> String> <- NEW Mapping()
mp[1] <- "Abali"
mp[2] <- "Tehran"
```