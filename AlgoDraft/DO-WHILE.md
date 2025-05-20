---
status: Planned
---
**Syntax:**

```
DO
	<body>
UNTIL <finish_1> EXIT
	<body_1>
UNTIL <finish_2> EXIT
	<body_2>
UNLESS <continue_a> EXIT
	<body_a>
UNLESS <continue_b> EXIT
	<body_b>
ENDDO
```

**Infinite Loop:**

```
DO
	//
UNTIL FALSE ENDDO

DO
	//
UNLESS TRUE ENDDO
```