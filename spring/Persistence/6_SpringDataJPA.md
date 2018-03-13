## Spring Data

### Paging

* page is a lightweight wrapper around a subset of sorted data

* to implement paging we need to know the order in which records should be returned

* we need to provide index and size of the page we want

* to add paging to spring data JPA we need to add a method like this to a repo:

```
@Query(value = "SELECT * FROM email WHERE business_id = ?1 ORDER BY ?#{#pageable}",
		   countQuery = "SELECT count(*) FROM email WHERE business_id = ?1 ORDER by ?#{#pageable}",
		   nativeQuery = true)
Page<Email> findByBusinessIdPagable(String businessId, Pageable pageable);
```

* from service we can call this like this:

```
public List<Email> findAllForBusinessAndPage(Business business, int index, int size) {
		return repo.findByBusinessIdPagable(
				business.getId(), 
				new PageRequest(
						index, 
						size, 
						new Sort( new Order(Direction.DESC, "sent")))) // how should items be ordered?
				.getContent();  // gets content of a Page
}
```
