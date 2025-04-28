# shield-pii Spring Boot library for detecting, marking & masking sensitive data



[![Build](https://img.shields.io/github/actions/workflow/status/your-org/shield-pii/ci.yml?branch=main)](...)

[![Maven Central](https://img.shields.io/maven-central/v/com.yourorg/shield-pii)](...)

[![License](https://img.shields.io/github/license/your-org/shield-pii)](LICENSE)



>  **Keep secrets secret.**

>  `shield-pii` automatically marks, masks and audits PII / PCI data in your Spring Boot applications before it ever reaches logs, JSON responses, stack traces or analytics sinks.



---



## ✨ Features



-  **Zero‑friction annotations** – `@Sensitive`, `@Mask(strategy = …)`, `@Redact` for Java records, POJOs and Lombok classes

-  **Flexible masking engine** – fixed‑length, last‑4, SHA‑256 hash, regex capture, reveal‑prefix/suffix, plus SPI for custom masks

-  **End‑to‑end coverage** – controllers, services, logs, exceptions, Actuator dumps, batch jobs

-  **Logback / Log4j 2 adapters** – TurboFilter and Lookup rewrite `%msg`, `%ex` and JSON appenders

-  **Jackson integration** – masks or redacts during JSON serialisation *and* deserialisation

-  **Secret detectors** – built‑in regex / algorithmic scanners for card numbers, PAN, Aadhaar, e‑mail, phone, JWT, API keys

-  **Observability guard** – Actuator endpoint `/actuator/shield-pii` lists unmasked sightings and performance stats

-  **Production‑grade performance** – byte‑code generation with ASM; < 50 µs per object, negligible GC impact at 10 k TPS



---



## 🚀 Quick Start



1.  **Add the dependency (Maven)**

```xml

<dependency>

<groupId>com.yourorg</groupId>

<artifactId>shield-pii-spring-boot-starter</artifactId>

<version>${shield-pii.version}</version>

</dependency>

```



*Using Gradle? Replace with an `implementation` line; coordinates are identical.*



2.  **Annotate a model**

```java

public  record  PaymentRequest(

@Sensitive(strategy = MaskingStrategy.CARD_LAST4) String cardNumber,

@Sensitive(strategy = MaskingStrategy.HASH_SHA256) String cvv,

@Sensitive  String cardholderName) {}

```



3.  **Run the app**

```java

log.info("Incoming {}", paymentRequest);

// → Incoming PaymentRequest(cardNumber=************4242, cvv=***, cardholderName=************)

```



---



## ⚙️ Configuration (`application.yml`)



```yaml

shield-pii:

log:

mask-all-logs: true

json:

fail-on-missing-annotation: false

regex:

enable-default-detectors: true

custom-strategies:

card-bin: "regex:(\d{6})(\d+)"  # keep first-6 BIN digits

```



---



## 📚 Usage Notes



* Create custom masks by implementing `MaskingStrategyProvider` or exposing a Spring `@Bean` of type `Function<Object,String>`.

* Logback / Log4j 2 filters are auto-installed; no XML edits required.

* Add `curl -f http://…/actuator/shield-pii?failOnUnmasked=true` to CI to block builds that leak data.

* Advanced topics (async pipelines, multi-module sharing, tuning) live in `/docs`.



## 🛣️ Roadmap



* OpenAPI / Swagger masking for example payloads

* gRPC interceptors for header & metadata masking

* Quarkus and Micronaut starters

* IDE plugin that highlights un‑annotated sensitive fields



---



## 🤝 Contributing



1. Fork & clone.

2.  `./mvnw verify` to run unit + integration tests.

3. Commit with **Conventional Commits**.

4. Open a pull request; GitHub Actions will run the pipeline.



First‑time contributors are welcome look for issues tagged **good‑first‑issue**.



---



## 📜 License



`shield-pii` is released under the **Apache 2.0 License**.

See the [LICENSE](LICENSE) file for full terms.



---