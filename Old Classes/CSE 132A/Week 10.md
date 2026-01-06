Cached Query plans
- Stores the best logical plan from previously run queries, so it can reuse it when the query is called again
	- Doesn't matter if the parameters are different

Partially Committed
- Computations are completed, but only a few of those results have been stored in disk/memory or what that task was meant to complete
- this will eventually fail unless the rest are completed directly after, since either you commit all or none. 