quick_sort:

	void quick_sort(int a[], int l, int r)
	{
		if (l >= r) 
			return;
	
		int m = l;
		for (int k = l + 1; k <= r; k++) 
		{
			if( a[k] <a[l])
			{
				++m;
				int temp = a[k]; a[k] = a[m]; a[m] = temp;
			}
		}
		int temp = a[l]; a[l] = a[m]; a[m] = temp;
	
		quick_sort(a, l, m - 1);
		quick_sort(a, m + 1, r);
	}


heap_sort:

	void fixDown(int a[], int k, int N)
	{
		int j;
		while (k * 2 <= N)
		{
			j = k * 2;
			if( j < N && a[j] < a[j+1]) j++;
			if( a[k] > a[j])break;
			int temp = a[k]; a[k] = a[j]; a[j] = temp;
			k = j;
		}
	}
	void heap_sort(int a[], int l, int r)
	{
		int k, N = r - l + 1;
		int *pq = a + l - 1;
		for ( k = N / 2; k >= 1; k--) 
		{
			fixDown(pq, k, N);
		}
		while (N > 1) 
		{
			int temp = pq[1];
			pq[1] = pq[N];
			pq[N] = temp;
			fixDown(pq, 1, --N);
		}
	}


