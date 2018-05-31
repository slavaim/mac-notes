
```
typedef struct pv_hashed_entry {
	/* first three entries must match pv_rooted_entry_t */
	queue_head_t		qlink;
	vm_map_offset_t		va_and_flags;
	pmap_t			pmap;
	ppnum_t			ppn;
	struct pv_hashed_entry	*nexth;
} *pv_hashed_entry_t;

....

pv_rooted_entry_t	pv_head_table;		/* array of entries, one per
						 * page */
             
...
             
/*
 *	Index into pv_head table, its lock bits, and the modify/reference and managed bits
 */

#define pa_index(pa)		(i386_btop(pa))
#define ppn_to_pai(ppn)		((int)ppn)

#define pai_to_pvh(pai)		(&pv_head_table[pai])

...
             
void
pmap_page_protect_options(
  ppnum_t         pn,
	vm_prot_t	prot,
	unsigned int	options,
	void		*arg)
{
	pv_hashed_entry_t	pvh_eh = PV_HASHED_ENTRY_NULL;
	pv_hashed_entry_t	pvh_et = PV_HASHED_ENTRY_NULL;
  
...

	pv_rooted_entry_t	pv_h;
	pv_rooted_entry_t	pv_e;
	pv_hashed_entry_t	pvh_e;
  
...

	pv_h = pai_to_pvh(pai);

	LOCK_PVH(pai);


	/*
	 * Walk down PV list, if any, changing or removing all mappings.
	 */
	if (pv_h->pmap == PMAP_NULL)
		goto done;

	pv_e = pv_h;
	pvh_e = (pv_hashed_entry_t) pv_e;	/* cheat */

	do {
		vm_map_offset_t vaddr;

		if ((options & PMAP_OPTIONS_COMPRESSOR_IFF_MODIFIED) &&
		    (pmap_phys_attributes[pai] & PHYS_MODIFIED)) {
			/* page was modified, so it will be compressed */
			options &= ~PMAP_OPTIONS_COMPRESSOR_IFF_MODIFIED;
			options |= PMAP_OPTIONS_COMPRESSOR;
		}

		pmap = pv_e->pmap;
		is_ept = is_ept_pmap(pmap);
		vaddr = PVE_VA(pv_e);
		pte = pmap_pte(pmap, vaddr);
...
		pvh_e = nexth;
	} while ((pv_e = (pv_rooted_entry_t) nexth) != pv_h);
...
}
```
