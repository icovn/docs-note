"thin controllers and fat models"

As a rule of thumb, you should follow the 5-10-20 rule, where controllers should only define 5 variables or less, contain 10 actions or less and include 20 lines of code or less in each action. This isn't an exact science, but it should help you realize when code should be refactored out of the controller and into a service.

Make your controller extend the FrameworkBundle base controller and use annotations to configure routing, caching and security whenever possible.



http://knpbundles.com/

http://knpbundles.com/genemu/GenemuFormBundle

http://knpbundles.com/dustin10/VichUploaderBundle

http://knpbundles.com/FriendsOfSymfony/FOSCommentBundle

http://knpbundles.com/FriendsOfSymfony/FOSElasticaBundle



\#unix

sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony

sudo chmod a+x /usr/local/bin/symfony 



\#windows

cd E:\HuyNQ28\OneDrive\study\symfony

SET HTTP\_PROXY=http://192.168.193.13:3128 && php -r "file\_put\_contents\('symfony', file\_get\_contents\('https://symfony.com/installer'\)\);" 



SET HTTP\_PROXY=http://192.168.193.13:3128 && composer install



E:\HuyNQ28\OneDrive\study\symfony\hello-symfony3

SET HTTP\_PROXY=http://192.168.193.13:3128 && composer create-project symfony/framework-standard-edition hello-symfony3

php bin/console server:run



git add --all

git commit -am "&lt;commit message&gt;"

git push



php bin/console security:check

php bin/console debug:router

php bin/console debug:router show\_slug

php bin/console router:match /lucky/my-latest-post

php bin/console debug:container



\#admin theme bundle

composer require avanzu/admin-theme-bundle

php bin/console assets:install --symlink

php bin/console avanzu:admin:fetch-vendor



\#user bundle

composer require friendsofsymfony/user-bundle "~2.0@dev"

php bin/console fos:user:create testuser quanghuy.ico@gmail.com 123456

php bin/console fos:user:create testuser

php bin/console fos:user:create adminuser --super-admin

php bin/console fos:user:promote testuser ROLE\_ADMIN



\#music bundle

php bin/console generate:bundle --namespace=Net/Friend/MusicBundle



\#generator-bundle

composer require sensio/generator-bundle



php bin/console doctrine:mapping:import --force MusicBundle xml

php bin/console doctrine:mapping:convert annotation ./src

php bin/console doctrine:generate:entities MusicBundle

php bin/console generate:doctrine:crud

php bin/console generate:doctrine:crud --entity=MusicBundle:Artist --format=annotation --with-write --no-interaction

php bin/console generate:doctrine:crud --entity=MusicBundle:Track --format=annotation --with-write --no-interaction



rm -rf var/cache/\*

rm -rf var/logs/\*



https://level7systems.co.uk/en/symfony2-admin-panel-in-30-seconds/



https://github.com/avanzu/AdminThemeBundle



\#diff between the 2 directories

diff -rq symfony-2.5.0/ symfony-3/



http://symfony.com/doc/current/best\_practices/index.html

http://symfony.com/doc/current/best\_practices/creating-the-project.html

http://symfony.com/doc/current/best\_practices/configuration.html

http://symfony.com/doc/current/best\_practices/business-logic.html

http://symfony.com/doc/current/best\_practices/controllers.html

http://symfony.com/doc/current/best\_practices/templates.html

http://symfony.com/doc/current/best\_practices/forms.html

http://symfony.com/doc/current/best\_practices/i18n.html

http://symfony.com/doc/current/best\_practices/security.html

http://symfony.com/doc/current/best\_practices/web-assets.html

http://symfony.com/doc/current/best\_practices/tests.html



http://symfony.com/doc/current/assetic/asset\_management.html

http://symfony.com/doc/current/assetic/uglifyjs.html

http://symfony.com/doc/current/assetic/jpeg\_optimize.html

https://github.com/kriswallsmith/assetic



http://symfony.com/doc/current/security.html



http://symfony.com/doc/current/components/event\_dispatcher.html

\#How to Set Up Before and After Filters

http://symfony.com/doc/current/event\_dispatcher/before\_after\_filters.html

http://symfony.com/doc/current/components/event\_dispatcher/traceable\_dispatcher.html

http://symfony.com/doc/current/components/event\_dispatcher/immutable\_dispatcher.html



http://symfony.com/doc/current/templating.html

http://symfony.com/doc/current/templating/app\_variable.html

http://symfony.com/doc/current/templating/debug.html

http://symfony.com/doc/current/templating/global\_variables.html

http://symfony.com/doc/current/templating/hinclude.html

http://symfony.com/doc/current/templating/overriding.html

http://symfony.com/doc/current/templating/syntax.html



http://symfony.com/doc/current/controller/upload\_file.html

http://symfony.com/doc/current/controller/error\_pages.html

http://symfony.com/doc/current/controller/service.html

http://symfony.com/doc/current/controller/soap\_web\_service.html



http://symfony.com/doc/current/configuration.html

\#How to Set external Parameters in the Service Container

http://symfony.com/doc/current/configuration/external\_parameters.html

http://symfony.com/doc/current/configuration/front\_controllers\_and\_kernel.html

https://en.wikipedia.org/wiki/Front\_controller



http://symfony.com/doc/current/doctrine.html

http://symfony.com/doc/current/doctrine/repository.html



http://symfony.com/doc/current/service\_container.html



http://symfony.com/doc/current/logging.html



http://symfony.com/doc/bundles/

http://symfony.com/doc/current/bundles/best\_practices.html

\#How to Load Service Configuration inside a Bundle

http://symfony.com/doc/current/bundles/extension.html

http://symfony.com/doc/current/bundles/inheritance.html

http://symfony.com/doc/current/bundles/configuration.html

http://symfony.com/doc/current/bundles/FOSUserBundle/index.html

http://symfony.com/doc/current/bundles/FOSUserBundle/user\_manager.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/index.html

http://symfony.com/doc/current/doctrine/reverse\_engineering.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_bundle.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_command.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_controller.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_doctrine\_crud.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_doctrine\_entity.html

http://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate\_doctrine\_form.html

http://symfony.com/doc/current/bundles/DoctrineFixturesBundle/index.html

http://symfony.com/doc/current/bundles/DoctrineMongoDBBundle/index.html

http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html

https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/converters.html

http://symfony.com/doc/current/bundles/SonataAdminBundle/index.html



http://symfony.com/doc/current/forms.html



\#How to Customize Form Rendering

http://symfony.com/doc/current/form/form\_customization.html



\#How to Change the Action and Method of a Form

http://symfony.com/doc/current/form/action\_method.html



\#How to Choose Validation Groups Based on the Clicked Button

http://symfony.com/doc/current/form/button\_based\_validation.html



\#How to Create a Custom Form Field Type

http://symfony.com/doc/current/form/create\_custom\_field\_type.html



\#How to Create a Form Type Extension

http://symfony.com/doc/current/form/create\_form\_type\_extension.html



\#How to Implement CSRF Protection

http://symfony.com/doc/current/form/csrf\_protection.html



\#How to Choose Validation Groups Based on the Submitted Data

http://symfony.com/doc/current/form/data\_based\_validation.html



\#How to Use Data Transformers

http://symfony.com/doc/current/form/data\_transformers.html



\#How to Dynamically Modify Forms Using Form Events

http://symfony.com/doc/current/form/dynamic\_form\_modification.html



\#Form Events

http://symfony.com/doc/current/form/events.html



\#How to Embed a Collection of Forms

http://symfony.com/doc/current/form/form\_collections.html



\#How to Customize Form Rendering

http://symfony.com/doc/current/form/form\_customization.html



\#How to Work with Form Themes

http://symfony.com/doc/current/form/form\_themes.html



\#How to Reduce Code Duplication with "inherit\_data"

http://symfony.com/doc/current/form/inherit\_data\_option.html



\#How to Unit Test your Forms

http://symfony.com/doc/current/form/unit\_testing.html



\#How to Upload Files

http://symfony.com/doc/current/controller/upload\_file.html



\#Caching Pages that Contain CSRF Protected Forms

http://symfony.com/doc/current/http\_cache/form\_csrf\_caching.html





\#New in Symfony 3.2: Unicode routing support

http://symfony.com/blog/new-in-symfony-3-2-unicode-routing-support



