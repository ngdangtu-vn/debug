ARG DDB_VER=1.4.3



### STAGE 3rd-party apps
FROM duckdb/duckdb:${DDB_VER} AS lake
# [distroless] empty stage for copying only



### STAGE builder 
FROM debian:bookworm-slim AS builder

# Install DuckDB extensions
COPY --from=lake /duckdb /usr/local/bin/duckdb
RUN duckdb \
    -c "INSTALL ducklake" \
    -c "INSTALL sqlite" \
    -c "INSTALL httpfs" \
    -c "FORCE INSTALL motherduck" \
    -c "INSTALL zipfs FROM community" \
    -c "FROM duckdb_extensions() WHERE install_path NOT LIKE '(%)' AND install_path != '';"

# Create volume entry /wh
RUN mkdir -p /wh/storage/



### STAGE final
FROM gcr.io/distroless/cc-debian12 AS final

# Input 
ARG DDB_VER
ENV H="/root"

# Image information
LABEL version="0.1" \ 
    description="A replicated image 'Segmentation fault' of DuckDB" \ 
    org.opencontainers.image.authors="biz@ngdangtu.dev"

# Create data directory
COPY --from=builder /wh /wh

# Install DuckDB
COPY --from=lake \
    /duckdb \
    /duckdb
COPY --from=builder \
    $H/.duckdb/extensions/v${DDB_VER} \
    $H/.duckdb/extensions/v${DDB_VER}
ENV PATH="$PATH:/"

CMD [ "/duckdb", "-ui" ]

