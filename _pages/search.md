---
layout: page
title: Search
---

<style>
	#search-container {
		margin: 20px 0;
	}

	#search-input {
		width: 100%;
		padding: 12px 16px;
		border: 2px solid #fd79a8;
		font-size: 16px;
		margin-bottom: 30px;
		background-color: #fef7f0;
		color: #333333;
		font-family: inherit;
		outline: none;
		-webkit-appearance: none;
		transition: all 0.2s ease;
	}

	#search-input:focus {
		border-color: #e84393;
		background-color: #ffffff;
		box-shadow: 0 0 0 3px rgba(232, 67, 147, 0.1);
	}

	#search-input::placeholder {
		color: #999999;
	}

	#results-container {
		list-style: none;
		padding: 0;
		margin: 0;
	}

	#results-container li {
		margin-bottom: 20px;
		padding: 16px;
		border: 2px solid #e8e8e8;
		transition: all 0.2s ease;
	}

	#results-container li:hover {
		border-color: #45b7d1;
		background-color: #f8fdff;
	}

	#results-container a {
		display: block;
		color: #333333;
		text-decoration: none;
		font-size: 18px;
		font-weight: 500;
		transition: color 0.2s ease;
	}

	#results-container a:hover {
		color: #45b7d1;
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