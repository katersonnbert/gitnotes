Database normalisation
======================

Distribution of data in columns (= attribute) and relational tables (= relation) to minimize redundancies.

Normal forms describe concepts to reduce specific cases of redundancies that can over time
lead to consistency errors in a database.

## First normal form (1NF)

Every attribute of a relation has to have an atomic domain (Wertebereich). In other words, every information that is
to be saved, has to be split up into its atomic domain and then assigned to its own attribute, otherwise
the information is not properly searchable.

e.g.

Name has to be split into first/middle/last name; address has to be split into zip code, city, street, number; etc.


## Second normal form (2NF)

Every attribute has to be part of the primary key OR has to be dependent on ALL parts of the primary key.
If an attribute is dependent on a subset of the primary key, redundancies and in turn data inconsistencies can arise.
Such attributes have to be moved to a secondary table and linked to the first one.

e.g.

Table customer
    ID   FN   LN           email_id  email
    101  Bob  Baumeister   1         b.b@workmail.com
    102  Karl Kalauer      1         k.k@workmail.com
    102  Karl Kalauer      2         k.k@kukmail.at

FN and LN are only dependent on ID, email is dependent on ID and email_id, FN and LN are becoming redundant in this schema.

Table customer
    ID   FN   LN
    101  Bob  Baumeister
    102  Karl Kalauer

Table customer_email
    ID   email_id  email
    101  1         b.b@workmail.com
    102  1         k.k@workmail.com
    102  2         k.k@kukmail.at

Every non primary key attribute is dependent on all primary key attributes of each table.


# Third normal form (3NF)

"Every non-key attribute must provide a fact about the key, the whole key and nothing but the key". A non key attribute
of a relation should be dependent ONLY on the primary key and not about a non key attribute. If an attribute can be
thematically interdependent on a non-key attribute, the dependent non-key attributes have to be moved to a different
table.

e.g.

Table books
    book_id     title                       author          author_birth_date
    101         The color from outer space  H.P.Lovecraft   20.08.1890
    102         Cthulhu                     H.P.Lovecraft   20.08.1890
    103         Der Golem                   Gustav Meyrink  19.01.1868

Author and author birth date in this context are both dependent on book_id, but are also dependent on each other
which can lead to data inconsistencies - birth date or author name could be updated independent from the other non-key
attribute on a single row, leading to a data inconsistency.

Table books
    book_id     title                       author
    101         The color from outer space  H.P.Lovecraft
    102         Cthulhu                     H.P.Lovecraft
    103         Der Golem                   Gustav Meyrink

Table authors
    author          author_birth_date
    H.P.Lovecraft   20.08.1890
    Gustav Meyrink  19.01.1868


## Q:
- What is DB normalisation
- What are normal forms in database schemas and why are they important.
- description 1NF, examples
- description 2NF, example
- description 3NF, example


## References
- [Wiki DB normalization (EN)](https://en.wikipedia.org/wiki/Database_normalization)
- [Wiki DB normalization (DE)](https://de.wikipedia.org/wiki/Normalisierung_%28Datenbank%29)
