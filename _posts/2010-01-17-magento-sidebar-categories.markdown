--- 
wordpress_id: 209
layout: post
title: Magento Sidebar Categories
date: 2010-01-17 23:06:37 -07:00
wordpress_url: http://www.bwigg.com/?p=209
---
The following will display a list of categories in your store in your right sidebar.
<h2>Layout</h2>
Alter page.xml so that the default handle's right block contains the following:

<pre lang="xml">
<block type="catalog/navigation" name="category.listing" template="catalog/navigation/categories.phtml" />
</pre>

My final right block looks as follows

<pre lang="xml">
<block type="core/text_list" name="right" as="right">
	<block type="catalog/navigation" name="featured" template="catalog/featured_random.phtml" />
	<block type="catalog/navigation" name="category.listing" template="catalog/navigation/categories.phtml" />
</block>
</pre>

<h2>Template</h2>
Create the file template/catalog/navigation/categories.phtml and paste in the following

<pre lang="php">
<div class="block block-layered-nav">
  <div class="block-title">
    <h2>Categories</h2>
  </div>
  <div class="block-content">
    <ul id="nav_category" class="nav_category">
    	<?php foreach ($this->getStoreCategories(true) as $_category): ?>
    		<?php echo $this->drawItem($_category) ?>
    	<?php endforeach ?>
    </ul>
  </div>
</div>
</pre>
