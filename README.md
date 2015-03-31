# Linear hash

An implementation of a linear probing hash table written in pure C99 with no external dependencies

Linear hash is licensed under the MIT license, see LICENSE for more details

Hashing function
----------------

linear_hash uses an implementation of djb2,
below is the reference implementation which can also be
found on [http://www.cse.yorku.ca/~oz/hash.html](http://www.cse.yorku.ca/~oz/hash.html)

    unsigned long
    hash(unsigned char *str)
    {
        unsigned long hash = 5381;
        int c;

        while (c = *str++)
            hash = ((hash << 5) + hash) + c; /* hash * 33 + c */

        return hash;
    }

Example usage
--------------

    #include "linear_hash.h"

    int main(void){
        /* create a hash with 32 buckets */
        struct lh_table *t = lh_new(32);

        /* some data to store */
        int data_1 = 1;
        int data_2 = 2;

        /* return pointer */
        int *data;

        /* insert new data */
        lh_insert(t, "hello", &data_1);
        lh_insert(t, "world", &data_2);

        /* fetch */
        data = lh_get(t, "hello");

        /* delete existing data */
        lh_delete(t, "world");

        /* mutate existing data */
        lh_set(t, "hello", &data_2);

        /* check a key exists */
        if( lh_exists(t, "hello") ){
        }

        /* resize hash */
        lh_resize(t, 64);

        /* tidy up
         * free table
         * but do not free stored data
         * destroy(table, free_table, free_data) */
        lh_destroy(t,     1,          0);
    }

