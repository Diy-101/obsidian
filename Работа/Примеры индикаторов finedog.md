search_request: pql sum(increase(http_server_requests_seconds_sum{env=~"ds-paym-prod1|m1-paym-prod1|ix-m2-paym-prod1|bm-ix-m4-paym-prod1|bm-ix-m5-paym-wl2|bm-ix-m4-inside-wl1|bm-ix-m5-inside-wl3|bm-ix-m5-inside-wl4", status!~"5..", uri=~"/api/v1/correction/.*/get_types"}[5m])) / sum(increase(http_server_requests_seconds_count{env=~"ds-paym-prod1|m1-paym-prod1|ix-m2-paym-prod1|bm-ix-m4-paym-prod1|bm-ix-m5-paym-wl2|bm-ix-m4-inside-wl1|bm-ix-m5-inside-wl3|bm-ix-m5-inside-wl4", status!~"5..", uri=~"/api/v1/correction/.*/get_types"}[5m]))  
req_interval: 1  
shift: 5  
slo: 2  
warning_slo: 1

search_request: pql step=1m histogram_quantile(0.99, sum(rate(http_server_requests_seconds_bucket{group="tpe-loyalty-api", uri=~"/api/v1/correction/.*/get_types", env=~"ds-paym-prod1|m1-paym-prod1|ix-m2-paym-prod1|bm-ix-m4-paym-prod1|bm-ix-m5-paym-wl2|bm-ix-m4-inside-wl1|bm-ix-m5-inside-wl3|bm-ix-m5-inside-wl4"}[5m])) by (le))  
req_interval: 1  
shift: 5  
slo: 0.3  
warning_slo: 0.25

search_request: pql step=1m (sum by (uri) (rate(http_server_requests_seconds_count{env=~"ds-paym-prod1|m1-paym-prod1|ix-m2-paym-prod1|bm-ix-m4-paym-prod1|bm-ix-m5-paym-wl2|bm-ix-m4-inside-wl1|bm-ix-m5-inside-wl4", uri=~"/api/v1/correction/.*/get_types", status=~"5..", status!="507"}[5m])) / sum by (uri) (rate(http_server_requests_seconds_count{env=~"ds-paym-prod1|m1-paym-prod1|ix-m2-paym-prod1|bm-ix-m4-paym-prod1|bm-ix-m5-paym-wl2|bm-ix-m4-inside-wl1|bm-ix-m5-inside-wl4", uri=~"/api/v1/correction/.*/get_types"}[5m]))) * 100 | stats last(value) as count  
req_interval: 1  
shift: 5  
slo: 10  
warning_slo: 5