annotation-target::remote-notes/PDF Files/Algebra and Relational Data Models.pdf





>%%
>```annotation-json
>{"created":"2025-10-02T16:35:44.219Z","text":"Notice that in the BarInfo table we have duplicate columns in bar and name. So the join should be followed by a theta join.\n\nTheta Join:\n- It's realistically just a cross product followed by a selection\n- The danger is that because we do cross product first, we can get a massive table even when our final table is small, thus its inefficient. \n","updated":"2025-10-02T16:35:44.219Z","document":{"title":"","link":[{"href":"urn:x-pdf:26cc6b8556193c419c2a6897b85724b5"},{"href":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf"}],"documentFingerprint":"26cc6b8556193c419c2a6897b85724b5"},"uri":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","target":[{"source":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","selector":[{"type":"TextPositionSelector","start":2840,"end":2846},{"type":"TextQuoteSelector","exact":"Theta ","prefix":" the name “theta-join.”Example: ","suffix":"JoinNatural Join•A useful join v"}]}]}
>```
>%%
>*%%PREFIX%%the name “theta-join.”Example:%%HIGHLIGHT%% ==Theta== %%POSTFIX%%JoinNatural Join•A useful join v*
>%%LINK%%[[#^74qrd6v9v2d|show annotation]]
>%%COMMENT%%
>Notice that in the BarInfo table we have duplicate columns in bar and name. So the join should be followed by a theta join.
>
>Theta Join:
>- It's realistically just a cross product followed by a selection
>- The danger is that because we do cross product first, we can get a massive table even when our final table is small, thus its inefficient. 
>
>%%TAGS%%
>
^74qrd6v9v2d


>%%
>```annotation-json
>{"created":"2025-10-02T16:47:39.884Z","text":"Particularly useful for renaming columns after performing joins and other operations.","updated":"2025-10-02T16:47:39.884Z","document":{"title":"","link":[{"href":"urn:x-pdf:26cc6b8556193c419c2a6897b85724b5"},{"href":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf"}],"documentFingerprint":"26cc6b8556193c419c2a6897b85724b5"},"uri":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","target":[{"source":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","selector":[{"type":"TextPositionSelector","start":3070,"end":3078},{"type":"TextQuoteSelector","exact":"Renaming","prefix":" := R1 ⨝ R2Example: Natural Join","suffix":"•The ρ operator gives a new sche"}]}]}
>```
>%%
>*%%PREFIX%%:= R1 ⨝ R2Example: Natural Join%%HIGHLIGHT%% ==Renaming== %%POSTFIX%%•The ρ operator gives a new sche*
>%%LINK%%[[#^wusokxv73yh|show annotation]]
>%%COMMENT%%
>Particularly useful for renaming columns after performing joins and other operations.
>%%TAGS%%
>
^wusokxv73yh


>%%
>```annotation-json
>{"created":"2025-10-02T17:07:59.071Z","text":"Refers to the tables having the same column names (attributes) and data type ","updated":"2025-10-02T17:07:59.071Z","document":{"title":"","link":[{"href":"urn:x-pdf:26cc6b8556193c419c2a6897b85724b5"},{"href":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf"}],"documentFingerprint":"26cc6b8556193c419c2a6897b85724b5"},"uri":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","target":[{"source":"vault:/Remote-Notes/PDF%20Files/Algebra%20and%20Relational%20Data%20Models.pdf","selector":[{"type":"TextPositionSelector","start":4033,"end":4041},{"type":"TextQuoteSelector","exact":"schemas ","prefix":"tersection, and difference: the ","suffix":"of the two operands must be the "}]}]}
>```
>%%
>*%%PREFIX%%tersection, and difference: the%%HIGHLIGHT%% ==schemas== %%POSTFIX%%of the two operands must be the*
>%%LINK%%[[#^27e0gqbi01l|show annotation]]
>%%COMMENT%%
>Refers to the tables having the same column names (attributes) and data type 
>%%TAGS%%
>
^27e0gqbi01l

Not on slides/PDF

Grouping Operations
- represented as $\gamma$ -> group by operation
- When to use?
	- Looking for key words like "per", "for every"
- active domain
	- All currently available values in a table
- Ex. group all Ds by As (D and A are column names)
	- Lets say A has an active domain of (a1, a2, b1), D has (3, 31, 5 ,6)
	- Computing:
		- Find all distinct values, then all the values for each distinct values, then perform the specified operation
			- a1 -> (3,31) group all D values where the A = a1, then the count is 2
			- a2 -> (4), count = 1
			- a3 -> (5,6), count = 2
		- By default of group by the operation would be count
	- If we have something like $\gamma CourseID:AVG(Grade)$ 
		- We do the same grouping thing where we get the list of values for each course, then instead of getting the count we perform the average operation.