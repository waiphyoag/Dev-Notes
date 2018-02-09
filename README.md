# JPA & Hibernate Notes

## When does Hibernate send updates to the database?
```java
@Transactional
void someMethodWithChange() {
    // Create Objects
    em.persist(user1);
    em.persist(user2);
    em.flush(); // Above changes are saved down to the database!
    // Change user1
    // Change user2
}
// All changes are saved down to the database!
```
## Why do we need @Transactional in Unit Tests sometimes?
If you want to do any changes to the database using jpa you will need a transaction.

> Unit Test => Repository => EntityManager
When we are indirecting through a repository (we are providing a transaction in the repository) that is no need @Transactional in Unit Test.

> Unit Test => EntityManager
Howerver, if we directly use the EntityManager in the unit test, then the unit test has to provide a transaction. (need @Transactional in Unit Test)
## Do read only methods need a transaction?
Entities - User, Comment
```java
List<Comment> someReadOnlyMethod() {
    User user = em.find(User.class, 1L); // SUCCESS! without @Transactioal
    // Because above line using EntityManager to retrieve the details 
    List<Comment> friends = user.getComments(); // FAIL! without @Transactional
    // Because above line use indirect way of retrieving relationship. So we need @Transactional.
    return friends;
}
```
## Why do we use @DirtiesContext in an Unit Test?
Prevent actual changes of database while testing.

## Relationship Fetching
> **ToOne => Default is Eager Fetching

> **ToMany => Default is Lazy Fetching

## Mapped By
Annotate in **not** owning side of the relationship
> OneToOne, ManyToMany => As you like

> OneToMany, ManyToOne => mappedBy is always on the **Many** side of the relationship (**One** side is the owning side)
