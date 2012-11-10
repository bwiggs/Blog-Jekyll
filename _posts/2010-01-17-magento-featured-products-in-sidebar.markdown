--- 
wordpress_id: 195
layout: post
title: Magento Sidebar Featured Products
date: 2010-01-17 22:59:18 -07:00
wordpress_url: http://www.bwigg.com/?p=195
---
This tutorial will explain how to add a featured product to the sidebar. In this example we will add the featured product to the right sidebar. The steps we will follow are as follows.
<ol>
	<li>Create a new category in the magento admin area to contain our featured products.</li>
	<li>Add the block calls to the XML Layout</li>
	<li>Create the phtml template files to be referenced by the block</li>
</ol>
<h2>Category</h2>
Add a new category to your store to contain your featured products. Make it inactive and jot down the id that it is given once saved. Add any products you want to consider featured to this category. Also make sure this category is not a root category.
<h2>Layout</h2>
Open page.xml and inside your default handle's right block add the following line.

<pre lang="xml"><block type="catalog/navigation" name="featured" template="catalog/featured_random.phtml" /></pre>

My finished right block looks as follows:

<pre lang="xml">
<block type="core/text_list" name="right" as="right">
	<block type="catalog/navigation" name="featured" template="catalog/featured_random.phtml" />
	<block type="catalog/navigation" name="category.listing" template="catalog/navigation/categories.phtml" />
</block>
</pre>

<h2>Template</h2>
Use the following code for featured_random.phtml

<pre lang="php"><?php
/**
* Magento
*
* NOTICE OF LICENSE
*
* This source file is subject to the Academic Free License (AFL 3.0)
* that is bundled with this package in the file LICENSE_AFL.txt.
* It is also available through the world-wide-web at this URL:
* http://opensource.org/licenses/afl-3.0.php
* If you did not receive a copy of the license and are unable to
* obtain it through the world-wide-web, please send an email
* to license@magentocommerce.com so we can send you a copy immediately.
*
* DISCLAIMER
*
* Do not edit or add to this file if you wish to upgrade Magento to newer
* versions in the future. If you wish to customize Magento for your
* needs please refer to http://www.magentocommerce.com for more information.
*
* @category   design_default
* @package    Mage
* @copyright  Copyright (c) 2008 Irubin Consulting Inc. DBA Varien (http://www.varien.com)
* @license    http://opensource.org/licenses/afl-3.0.php  Academic Free License (AFL 3.0)
*/
?>

<?php 
$category_id = "25"; // category_id for "Featured Products"
$_productCollection = Mage::getResourceModel('catalog/product_collection')
  ->addAttributeToSelect(array('name', 'price', 'small_image'), 'inner')
  ->addCategoryFilter(Mage::getModel('catalog/category')->load($category_id));
?>

<?php if($_productCollection->count()): ?>
  
  <?php 
  $products = array();
  foreach ($_productCollection as $_product) { array_push($products, $_product); }
  $_product = $products[rand(0,count($products)-1)];
  ?>
<div class="block block-featured-product">
  <div class="block-title">
    <h2>Featured Product</h2>
  </div>
  <div class="block-content">
    <ul id="featured-product-list">
      <li class="featured-product">
          <h4><?php echo $this->htmlEscape($_product->getName())?></h4>
          <a href="<?php echo $_product->getProductUrl() ?>" title="<?php echo $this->htmlEscape($this->getImageLabel($_product, 'small_image')) ?>">
            <img class="product-image" src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->resize(117, 117); ?>" alt="<?php echo $this->htmlEscape($this->getImageLabel($_product, 'small_image')) ?>" title="<?php echo $this->htmlEscape($this->getImageLabel($_product, 'small_image')) ?>" />
          </a>
          <a class="view-item-button" href="<?php echo $_product->getProductUrl() ?>" title="<?php echo $this->htmlEscape($_product->getName()) ?>">View Item</a>
      </li>
    </ul>
  </div>
</div>
<?php endif; ?></pre>

<strong>Resources:</strong>
<ul>
	<li>Elias Interactive - Adapted from <a title="Homepage Featured Products" href="http://www.eliasinteractive.com/blog/magento-featured-products-a-more-convenient-way-to-display-featured-products-on-the-home-page" target="_blank">Homepage Featured Products</a></li>
</ul>
