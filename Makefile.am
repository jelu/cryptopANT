lib_LTLIBRARIES = libcryptopant.la
libcryptopant_la_SOURCES = src/cryptopANT.c src/cryptopANT.h
libcryptopant_la_LDFLAGS = -version-info @CRYPTOPANT_LIBRARY_VERSION@ -lcrypto
ACLOCAL_AMFLAGS = -I m4
include_HEADERS = src/cryptopANT.h
dist_man3_MANS = man/cryptopANT.3

if BUILD_SCRAMBLE_IPS
# these will be installed in BINDIR
bin_PROGRAMS = scramble_ips
scramble_ips_SOURCES = src/scramble_ips.c
scramble_ips_LDFLAGS = -L.libs -lcryptopANT
$(bin_PROGRAMS): $(lib_LTLIBRARIES)
endif

.PHONY: test
TEST_BINARY = .libs/scramble_ips
test: all scramble_ips test/keyfile test/ip4s test/ip6s test/ip4s.anon test/ip6s.anon
	@$(TEST_BINARY) test/keyfile <test/ip4s | cmp -l test/ip4s.anon - \
		&& echo "ipv4 scrambling test PASSED" \
		|| echo "ipv4 scrambling test FAILED"
	@$(TEST_BINARY) test/keyfile <test/ip6s | cmp -l test/ip6s.anon - \
		&& echo "ipv6 scrambling test PASSED" \
		|| echo "ipv6 scrambling test FAILED"
	@$(TEST_BINARY) -r test/keyfile <test/ip4s.anon | cmp -l test/ip4s - \
		&& echo "ipv4 descrambling test PASSED" \
		|| echo "ipv4 descrambling test FAILED"
	@$(TEST_BINARY) -r test/keyfile <test/ip6s.anon | cmp -l test/ip6s - \
		&& echo "ipv6 descrambling test PASSED" \
		|| echo "ipv6 descrambling test FAILED"

.PHONY: rpmbuild
rpmbuild: dist
	cp $(distdir).tar.gz ~/rpmbuild/SOURCES
	cp cryptopANT.spec   ~/rpmbuild/SPECS
	rpmbuild -ba ~/rpmbuild/SPECS/cryptopANT.spec
