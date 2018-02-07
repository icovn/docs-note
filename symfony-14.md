\#\#\#symfony projects

\#Sismo: Your Continuous Testing Server http://sismo.sensiolabs.org

https://github.com/FriendsOfPHP/Sismo



\#Goutte: Goutte is a screen scraping and web crawling library for PHP

https://github.com/FriendsOfPHP/Goutte



\#OroCRM:

https://github.com/orocrm/crm-application

https://github.com/orocrm/platform



\#Sylius: eCommerce PHP framework built on top of Symfony with component-based architecture and format-agnostic rendering

https://github.com/Sylius/Sylius



\#akeneo: The open source Product Information Management \(PIM\) 

https://github.com/akeneo/pim-community-dev



\#Shopware: the next generation of open source e-commerce software made in Germany. Based on bleeding edge technologies like Symfony 2, Doctrine 2 & Zend Framework

https://github.com/shopware/shopware



\# Mautic: Open Source Marketing Automation Software

https://github.com/mautic/mautic



https://www.drupal.org/8

https://ezplatform.com/

https://github.com/ezsystems/ezplatform

https://doc.ez.no/display/DEVELOPER/Documentation

\#\#\#symfony-1.4----------------------------------------------------------------------------------------------------------------------------------------------------------------------

enabled

Default: false

The enabled setting enables or disables the cache for the current scope.



with\_layout

Default: false

The with\_layout setting determines whether the cache must be for the entire page \(true\), or for the action only \(false\).

The with\_layout option is not taken into account for partial and component caching as they cannot be decorated by a layout.



lifetime

Default: 86400

The lifetime setting defines the server-side lifetime of the cache in seconds \(86400 seconds equals one day\).



client\_lifetime

Default: Same value as the lifetime one

The client\_lifetime setting defines the client-side lifetime of the cache in seconds.

This setting is used to automatically set the Expires header and the max-cache cache control variable, unless a Last-Modified or Expires header has already been set.

You can disable client-side caching by setting the value to 0.



contextual

Default: false

The contextual setting determines if the cache depends on the current page context or not. The setting is therefore only meaningful when used for partials and components.

When a partial output is different depending on the template in which it is included, the partial is said to be contextual, and the contextual setting must be set to true. By default, the setting is set to false, which means that the output for partials and components are always the same, wherever it is included.

The cache is still obviously different for a different set of parameter

