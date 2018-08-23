The max path length for which the kernel can create a vnode is defined as ```MAXPATHLEN```(i.e. ```1024```) by code in ```namei()```. Note that ```PATHBUFLEN < MAXPATHLEN``` (essentially ```PATHBUFLEN``` is ```256``` for the latest kernel at the time of writing)

```
int
namei(struct nameidata *ndp)
{
...
      /*
       * Get a buffer for the name to be translated, and copy the
       * name into the buffer.
       */
      if ((cnp->cn_flags & HASBUF) == 0) {
        cnp->cn_pnbuf = ndp->ni_pathbuf;
        cnp->cn_pnlen = PATHBUFLEN;
      }
    #if LP64_DEBUG
      if ((UIO_SEG_IS_USER_SPACE(ndp->ni_segflg) == 0)
        && (ndp->ni_segflg != UIO_SYSSPACE)
        && (ndp->ni_segflg != UIO_SYSSPACE32)) {
        panic("%s :%d - invalid ni_segflg\n", __FILE__, __LINE__); 
      }
    #endif /* LP64_DEBUG */

    retry_copy:
      if (UIO_SEG_IS_USER_SPACE(ndp->ni_segflg)) {
        error = copyinstr(ndp->ni_dirp, cnp->cn_pnbuf,
              cnp->cn_pnlen, &bytes_copied);
      } else {
        error = copystr(CAST_DOWN(void *, ndp->ni_dirp), cnp->cn_pnbuf,
              cnp->cn_pnlen, &bytes_copied);
      }
      if (error == ENAMETOOLONG && !(cnp->cn_flags & HASBUF)) {
        MALLOC_ZONE(cnp->cn_pnbuf, caddr_t, MAXPATHLEN, M_NAMEI, M_WAITOK);
        if (cnp->cn_pnbuf == NULL) {
          error = ENOMEM;
          goto error_out;
        }

        cnp->cn_flags |= HASBUF;
        cnp->cn_pnlen = MAXPATHLEN;
        bytes_copied = 0;

        goto retry_copy;
      }
...
}
```
