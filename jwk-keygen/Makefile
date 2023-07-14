DATE=$(shell date "+%Y%m%d")
PROJECT=project
SIG_ALG=ES256
ENC_ALG=ECDH-ES+A128KW

# default action when running `make`
run: generate jsonify

#

generate: jwk_sig_$(SIG_ALG)_sig-$(PROJECT)-$(DATE).pub jwk_enc_$(ENC_ALG)_enc-$(PROJECT)-$(DATE).pub

jsonify: $(PROJECT)-$(DATE)-secret.json $(PROJECT)-$(DATE)-public.json
	ls $(PROJECT)-$(DATE)-*.json

copy-public:
	jq -c < $(PROJECT)-$(DATE)-public.json | pbcopy

copy-secret:
	jq -c < $(PROJECT)-$(DATE)-secret.json | pbcopy

#

jwk_sig_$(SIG_ALG)_sig-$(PROJECT)-$(DATE).pub:
	go run main.go --use sig --alg $(SIG_ALG) --kid sig-$(PROJECT)-$(DATE)

jwk_enc_$(ENC_ALG)_enc-$(PROJECT)-$(DATE).pub:
	go run main.go --use enc --alg $(ENC_ALG) --kid enc-$(PROJECT)-$(DATE)

$(PROJECT)-$(DATE)-secret.json:
	((echo '{"keys":['; for x in *$(PROJECT)-$(DATE); do cat $$x; echo ''; echo ','; done) | sed '$$d'; printf "]\n}\n")  | jq | tee $(PROJECT)-$(DATE)-secret.json

$(PROJECT)-$(DATE)-public.json:
	((echo '{"keys":['; for x in *$(PROJECT)-$(DATE).pub; do cat $$x; echo ''; echo ','; done) | sed '$$d'; printf "]\n}\n")  | jq | tee $(PROJECT)-$(DATE)-public.json

clear:
	rm -f $(PROJECT)-$(DATE)-*.json *$(PROJECT)-$(DATE)*
