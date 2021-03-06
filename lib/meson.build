flags = '-Wl,--version-script=' + meson.current_source_dir() + '/libjose.map'
code = 'int main() { return 0; }'
cc = meson.get_compiler('c')

if not cc.links(code, args: flags, name: '-Wl,--version-script=...')
  flags = [ '-export-symbols-regex=^jose_.*' ]
endif

libjose_sources = [
  'misc.c',           'misc.h',
  'cfg.c',
  'io.c',
  'b64.c',
  'hsh.c',            'hsh.h',
  'hooks.c',          'hooks.h',
  'jwk.c',
  'jws.c',
  'jwe.c',
  'zlib/deflate.c',
  'openssl/aescbch.c',
  'openssl/aesgcm.c',
  'openssl/aesgcmkw.c',
  'openssl/aeskw.c',
  'openssl/compat.c', 'openssl/compat.h',
  'openssl/dir.c',
  'openssl/ec.c',
  'openssl/ecdh.c',
  'openssl/ecdhes.c',
  'openssl/ecmr.c',
  'openssl/ecdsa.c',
  'openssl/hash.c',
  'openssl/hmac.c',
  'openssl/jwk.c',
  'openssl/lock.c',
  'openssl/misc.c',   'openssl/misc.h',
  'openssl/oct.c',
  'openssl/pbes2.c',
  'openssl/rsa.c',
  'openssl/rsaes.c',
  'openssl/rsassa.c',
]

if cc.compiles('''
    #include <tls.h>
    int main(void) {
        tls_config_set_ca_mem(NULL, NULL, 0);
        return 0;
    }
''')
   libjose_sources += ['openssl/libressl_evp.c']
endif


libjose = library('jose',
  libjose_sources,
  dependencies: [zlib, jansson, libcrypto, threads],
  version: '0.0.0',
  link_args: flags,
  install: true
)

