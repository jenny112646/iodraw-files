```mermaid
flowchart TD
    start([b2n_block_init]) --> b2n_get_sblock
    subgraph b2n_get_sblock
        subgraph pu_mgr.init_block_dist
            start_b["update xlt_info & get bad plane info"] --> a
            a[chk all plane die] --> b{"if ALL_BAD_PLANE || 
            (PF stream & HAS_BAD_PLANE)"}
            b --yes--> c[add total bad plane]
            b --no--> d{"chk if NO_BAD_PLANE"}
            c --> e[continue to next die]
            d --yes--> f["find_good_die = TRUE
            bad_plane_num = 0"]
            d --no--> g[update bad_plane_num & mask]
            g --> h{"if require_raid_start"}
            f --> h
            h --yes--> i["set cmd RAID_START 
            set XPAGE_UP
            require_raid_start = FALSE"]
            h --"no"--> j{"if die % PARITY_RATIO 
            < STRIPE_DFT_SIZE"}
            j --yes--> k["set cmd RAID_XOR
            set XPAGE_UP"]
            j --no--> l["set cmd RAID_XOR
            set XPAGE_DOWN"]
            k --> m["xlat_seq_die ++"]
            l --> m
            m --> n{"if (die % PARITY_RATIO) 
            == (PARITY_RATIO - 1)
            || die == end_die"}
            n --> 

             

        end
    end
```