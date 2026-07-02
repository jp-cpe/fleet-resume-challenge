# 4. GitOps

Manage Fleet as code with GitOps.

> **GitOps** is available on the free tier; per-fleet GitOps config requires **fleets** (Premium — free with the trial from [Module 1](1-deploy-fleet.md)).

## Goal
Your live Fleet configuration captured in a Git repo, with changes applied automatically via CI/CD and GitOps mode locking the UI from drift.

## Resources
- [Step-by-step: GitOps](../guides/gitops.md)
- [Fleet GitOps documentation](https://fleetdm.com/docs/configuration/yaml-files)
- [Migrating to GitOps using fleetctl](https://fleetdm.com/guides/migrating-to-gitops-using-fleetctl)
- [Fleet's internal GitOps repo](https://github.com/fleetdm/fleet/tree/main/it-and-security)
- 🎓 [Attend a Fleet GitOps workshop](https://fleetdm.com/gitops-workshop)

## Tasks
- [ ] Export your existing Fleet config to GitOps YAML with `fleetctl generate-gitops`
- [ ] Commit the config to a git repo with a workflow that runs `fleetctl gitops`
- [ ] Confirm pushes to main apply changes while PRs run dry-run only
- [ ] Enable GitOps mode to lock the UI from drift
- [ ] **Audit for secrets:** ensure all `# TODO` placeholders are filled via GitHub Actions secrets (not hardcoded), and consider adding a gitleaks pre-commit hook

## Done when
- [ ] Your live config is captured in Git, a commit to main updates Fleet within ~1 min, GitOps mode is on, and secrets are stored only in GitHub Actions secrets (never in YAML)

## ⚠️ Secrets hygiene

Since you'll link this repo publicly on your resume (see [Putting Fleet on your resume](../guides/resume.md)), **protect your credentials before pushing.** See the ["Secrets hygiene" section](../guides/gitops.md#️-secrets-hygiene-critical-before-making-the-repo-public) of the step-by-step guide for detailed instructions on storing secrets safely and scanning for accidental commits.
