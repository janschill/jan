---
layout: page
title: Search
---

<style>
	#search-container {
		margin: 16px 0;
	}

	#search-input {
		width: 100%;
		padding: 12px 16px;
		border: 3px solid var(--border);
		box-shadow: 3px 3px 0 var(--border);
		font-size: 16px;
		font-weight: 600;
		margin-bottom: 24px;
		background-color: var(--bg);
		color: var(--text);
		font-family: inherit;
		outline: none;
		-webkit-appearance: none;
		transition: all 0.1s ease;
	}

	#search-input:focus {
		transform: translate(1px, 1px);
		box-shadow: 2px 2px 0 var(--border);
	}

	#search-input::placeholder {
		color: var(--gray-mid);
	}

	#results-container {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	#results-container li {
		margin-bottom: 12px;
		padding: 14px 16px;
		border: 3px solid var(--border);
		box-shadow: 3px 3px 0 var(--border);
		background-color: var(--bg);
		transition: all 0.1s ease;
	}

	#results-container li:hover {
		transform: translate(1px, 1px);
		box-shadow: 2px 2px 0 var(--border);
	}

	#results-container a {
		display: block;
		color: var(--text);
		text-decoration: none;
		font-size: 16px;
		font-weight: 600;
	}

	#results-container a:hover {
		color: var(--accent-dark);
	}

	@media (min-width: 600px) {
		#search-input {
			box-shadow: 4px 4px 0 var(--border);
		}

		#search-input:focus {
			transform: translate(2px, 2px);
		}

		#results-container li {
			box-shadow: 4px 4px 0 var(--border);
		}

		#results-container li:hover {
			transform: translate(2px, 2px);
		}
	}
</style>

<!-- Html Elements for Search -->
<div id="search-container">
<input type="text" id="search-input" placeholder="Search blog posts...">
<ol id="results-container"></ol>
</div>

<!-- Script pointing to search-script.js -->
<script src="/search.js" type="text/javascript"></script>

<!-- Configuration -->
<script type="text/javascript">
SimpleJekyllSearch({
  searchInput: document.getElementById('search-input'),
  resultsContainer: document.getElementById('results-container'),
  json: '/search.json',
  searchResultTemplate: '<li><a href="{url}" title="{description}">{title}</a></li>',
  noResultsText: 'No results found',
  limit: 10,
  fuzzy: false,
  exclude: ['Welcome']
})
</script>