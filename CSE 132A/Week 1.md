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
