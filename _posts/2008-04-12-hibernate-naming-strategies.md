---
ID: 20
post_title: Hibernate Naming Strategies
author: matthew
post_date: 2008-04-12 00:09:42
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/hibernate-naming-strategies-20.html
published: true
btc_comment_summary:
  - 'a:0:{}'
btc_comment_counts:
  - 'a:0:{}'
dsq_thread_id:
  - "161305393"
---
Using <a href="http://www.hibernate.org/397.html">Hibernate annotations</a> with the default naming strategy leaves you with camelCasedColumnNames in your database schema. 

Gavin King provided a good camelCase to underscore_separated <a href="http://www.hibernate.org/hib_docs/v3/reference/en/html/session-configuration.html#configuration-namingstrategy">naming strategy</a> with <a href="http://www.hibernate.org/hib_docs/v3/api/org/hibernate/cfg/ImprovedNamingStrategy.html">org.hibernate.cfg.ImprovedNamingStrategy</a>. The only glitch I found was in foreign key references. I've always seen the naming convention of <code>${tablename}_id</code>, but ImprovedNamingStrategy just called the column the same name as any other field. That's easily overridden in a subclass:

[code='java']
public class NewAndImprovedNamingStrategy extends ImprovedNamingStrategy {
    @Override
    public String foreignKeyColumnName(String propertyName, String propertyEntityName, String propertyTableName, String referencedColumnName) {
        String s = super.foreignKeyColumnName(propertyName, propertyEntityName, propertyTableName, referencedColumnName);
        return s.endsWith("_id") ? s : s + "_id";
    }
}
[/code]

If you're using Spring, wire it up to your SessionFactoryBean:

[code='xml']
  <bean id="namingStrategy" class="...NewAndImprovedNamingStrategy"/>
  ...
  <bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
    <property name="namingStrategy" ref="namingStrategy"/>
    ...
[/code]

And you'll be freed from doing boilerplate "<code>name=</code>" attributes in your domain POJOs.