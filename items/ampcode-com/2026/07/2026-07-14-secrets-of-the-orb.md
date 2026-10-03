---
title: Secrets of the Orb
link: https://ampcode.com/news/secrets-of-the-orb
source: ampcode-com
published: 2026-07-14T00:00:00Z
updated: 2026-07-14T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Orbs can now prove their identity using OIDC, so you can use Amp for ops and infrastructure tasks more safely. Service providers that support OIDC authentication can be configured to trust the identity provided by Amp and grant permissions to your orb based on that identity, without having to inject any secrets into orbs. For example, we''ve configured GCP to grant orbs in our workspace read access to our production logs. This requires zero config from any of our devs, any operations are fully auditable because they include claims like user ID and thread ID, and the tokens are all short-lived and tightly scoped to the operations that project needs. To mint a token, run amp orb id-token --audience <aud> in your orb: ~/workspace/repo$ jwt decode -j $(amp orb id-token --audience test) { "header": { "typ": "JWT", "alg": "RS256", "kid": "5A4Ie1hntIuzu4KtaDPXr2HhnButOliWxM9ncVsct1s" }, "payload": { "aud": "test", "email": "user@example.com", "email_verified": true, "exp": 1784078965, "iat": 1784078365, "iss": "https://ampcode.com/api/workload-identity", "jti": "78bceb61-4102-4973-8bec-d05d65be4b67", "project_id": "12038eda-75e9-46fa-b185-f67404f40b2f", "sub": "workspace:0284a29c-b50a-41f8-b83f-94e5c9b32f5f:project:12038eda-75e9-46fa-b185-f67404f40b2f:user:user_3V306PAAABM94KT1N05NSDQRBD:thread:T-6600f272-b20f-4652-bb6a-3a3c73099d31", "thread_id": "T-6600f272-b20f-4652-bb6a-3a3c73099d31", "token_use": "exchanged", "user_id": "user_3V306PAAABM94KT1N05NSDQRBD", "workspace_id": "0284a29c-b50a-41f8-b83f-94e5c9b32f5f" } } Alongside standard claims like iss, aud, sub, and exp, Amp adds custom claims such as workspace_id, project_id, user_id, thread_id, and email. Okay, but what can I actually do with that token? Even after building OIDC into orbs, it wasn''t immediately obvious to me how far we could push an OIDC-based dev env. So here are a few ideas to get the juices flowing, drawn from both internal usage and feedback from our early partners. Configure workload identity federation in GCP and grant access to your production logs and metrics from all orbs in your workspace. Add OIDC trust credentials to your Tailscale workspace and grant users on your tailnet the ability to hit services running in orbs so you can share demo apps with your colleagues. Commit your SOPS-encrypted dev secrets to your repo, then grant your users'' orbs access to decrypt those secrets with AWS KMS so you don''t need to maintain 50 secret environment variables in your project settings to run your dev environment. Create a PAM entitlement that grants escalated privileges to this specific orb, and add a script to your repo that allows your Amp thread to request an escalation. Spin up an oauth2-proxy instance to inject credentials into requests to services that don''t support OIDC auth. Want more help getting started? Head over to the manual for detailed recipes.'
content: extracted
html: 2026-07-14-secrets-of-the-orb.html
preview:
  file: 2026-07-14-secrets-of-the-orb.preview-af725c5ed2f8.webp
  width: 256
  height: 134
  color: '#5d564c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Secrets+of+the+Orb&date=July+14%2C+2026&tagline=Orbs+now+support+OIDC+for+more+secure+and+granular+workload+identity&sig=e369485827dd5daf9e274c9aacbc75c22b8267a4f9a9cd76872619c6d4b85071
  original:
    file: 2026-07-14-secrets-of-the-orb.image-ee7abb66e359.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-14-secrets-of-the-orb.image-aaf617158768.webp
    width: 320
    height: 168
  - file: 2026-07-14-secrets-of-the-orb.image-00d599a5b7d5.webp
    width: 640
    height: 336
  - file: 2026-07-14-secrets-of-the-orb.image-93e7b62883dd.webp
    width: 960
    height: 504
  - file: 2026-07-14-secrets-of-the-orb.image-354ee6c7ef06.webp
    width: 1200
    height: 630
  color: '#221b16'
---

Orbs can now prove their identity using [OIDC](https://openid.net/developers/how-connect-works/), so you can use Amp for ops and infrastructure tasks more safely.

Service providers that support OIDC authentication can be configured to trust the identity provided by Amp and grant permissions to your orb based on that identity, *without* having to inject any secrets into orbs.

For example, we've configured GCP to grant orbs in our workspace read access to our production logs. This requires zero config from any of our devs, any operations are fully auditable because they include claims like user ID and thread ID, and the tokens are all short-lived and tightly scoped to the operations that project needs.

To mint a token, run `amp orb id-token --audience <aud>` in your orb:

```bash
~/workspace/repo$ jwt decode -j $(amp orb id-token --audience test)
{
	"header": {
		"typ": "JWT",
		"alg": "RS256",
		"kid": "5A4Ie1hntIuzu4KtaDPXr2HhnButOliWxM9ncVsct1s"
	},
	"payload": {
		"aud": "test",
		"email": "user@example.com",
		"email_verified": true,
		"exp": 1784078965,
		"iat": 1784078365,
		"iss": "https://ampcode.com/api/workload-identity",
		"jti": "78bceb61-4102-4973-8bec-d05d65be4b67",
		"project_id": "12038eda-75e9-46fa-b185-f67404f40b2f",
		"sub": "workspace:0284a29c-b50a-41f8-b83f-94e5c9b32f5f:project:12038eda-75e9-46fa-b185-f67404f40b2f:user:user_3V306PAAABM94KT1N05NSDQRBD:thread:T-6600f272-b20f-4652-bb6a-3a3c73099d31",
		"thread_id": "T-6600f272-b20f-4652-bb6a-3a3c73099d31",
		"token_use": "exchanged",
		"user_id": "user_3V306PAAABM94KT1N05NSDQRBD",
		"workspace_id": "0284a29c-b50a-41f8-b83f-94e5c9b32f5f"
	}
}
```

Alongside standard claims like `iss`, `aud`, `sub`, and `exp`, Amp adds custom claims such as `workspace_id`, `project_id`, `user_id`, `thread_id`, and `email`.

## Okay, but what can I actually *do* with that token?

Even after building OIDC into orbs, it wasn't immediately obvious to me how far we could push an OIDC-based dev env. So here are a few ideas to get the juices flowing, drawn from both internal usage and feedback from our early partners.

1. Configure [workload identity federation](https://docs.cloud.google.com/iam/docs/workload-identity-federation) in GCP and grant access to your production logs and metrics from all orbs in your workspace.
2. Add OIDC [trust credentials](https://tailscale.com/docs/reference/trust-credentials) to your Tailscale workspace and grant users on your tailnet the ability to hit services running in orbs so you can share demo apps with your colleagues.
3. Commit your [SOPS-encrypted dev secrets](https://getsops.io/docs/usage/first-steps/) to your repo, then grant your users' orbs access to [decrypt those secrets with AWS KMS](https://getsops.io/docs/usage/identities/amazon-aws-kms/) so you don't need to maintain 50 secret environment variables in your project settings to run your dev environment.
4. Create a [PAM entitlement](https://docs.cloud.google.com/iam/docs/pam-overview) that grants escalated privileges to this specific orb, and add a script to your repo that allows your Amp thread to request an escalation.
5. Spin up an [oauth2-proxy instance](https://github.com/oauth2-proxy/oauth2-proxy) to inject credentials into requests to services that don't support OIDC auth.

Want more help getting started? Head over to [the manual](https://ampcode.com/manual/orbs/oidc) for detailed recipes.
