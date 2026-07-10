# TN0702 Bucket

A **Bucket** is one of the pair of public-read Aliyun OSS buckets owned by a
[Project](TN0301_project.md) — `<identifier>-staging` and `<identifier>-production` — to which
the generated static site is deployed and from which it is served. The pair is described by the
`BucketType` enum in the `lib/oss` module: a [Deploy Task](TN0701_deploy_task.md)'s phase is
mapped to a `BucketType` (via `DeployPhase.bucketType`), and the bucket name is derived from the
project's [Identifier](TN0101_identifier.md). Uploads during a deployment are performed through
the `OssDeployer` class; bucket lifecycle operations (create / delete / website configuration)
are provided by the `Oss` object.

## Code mapping

| Class | DB table | Source |
|---|---|---|
| `BucketType` (enum, `lib/oss`) | — (not persisted; names are derived from the project identifier) | [BucketType.kt](/source/pager-backend/lib/oss/src/main/kotlin/com/xwkj/pager/oss/BucketType.kt) |
| `OssDeployer` (class, `lib/oss`) | — | [OssDeployer.kt](/source/pager-backend/lib/oss/src/main/kotlin/com/xwkj/pager/oss/OssDeployer.kt) |
| `Oss` (object, `lib/oss`) | — | [Oss.kt](/source/pager-backend/lib/oss/src/main/kotlin/com/xwkj/pager/oss/Oss.kt) |

## Important fields

`BucketType` has no persisted fields; its two values name the buckets through the
`bucketName(projectIdentifier: String)` function:

### Values — enum `BucketType`

| Value | `bucketName(projectIdentifier)` |
|---|---|
| `STAGING` | `"$projectIdentifier-staging"` |
| `PRODUCTION` | `"$projectIdentifier-production"` |

### Bucket properties

Both values return the same computed properties, applied when the bucket is created
(`Oss.createBucket`):

| Property | Value | Meaning |
|---|---|---|
| `storageClass` | `StorageClass.Standard` | Standard OSS storage class. |
| `dataRedundancyType` | `DataRedundancyType.LRS` | Locally redundant storage. |
| `cannedACL` | `CannedAccessControlList.PublicRead` | The bucket is publicly readable, so the deployed site is served directly from OSS. |

### `OssDeployer`

`OssDeployer(bucket, credential)` wraps one OSS client bound to a single bucket name and an
`OssCredential` (region + access-key pair), and exposes `uploadText(pathname, content)`,
`uploadFile(pathname, file)`, and `shutdown()`. During a deployment the consumer constructs it
as `OssDeployer(bucket = deployTask.phase.bucketType.bucketName(project.identifier), ...)` with
a credential built from the project's `region` and its [Access Key](TN0204_access_key.md).

### Lifecycle helpers — `Oss`

- `Oss.createBucket(projectIdentifier, credential)` creates **both** buckets with the properties
  above, and only when neither bucket name already exists (returns `false` otherwise).
- `Oss.setBucketWebsite(...)` configures both buckets' static-website settings with the
  project's `indexDocument` / `errorDocument` and sub-directory redirect support.
- `Oss.deleteBucket(projectIdentifier, credential)` removes whichever of the two buckets exist.

## Relationships

- **[Project](TN0301_project.md)** — the project's `identifier` yields the two bucket names
  (`1` project → `2` buckets, one per `BucketType` value); the project's `region` selects the
  OSS endpoint through `OssCredential`, and its `indexDocument` / `errorDocument` become the
  buckets' website configuration.
- **[Deploy Task](TN0701_deploy_task.md)** — the task's `phase` maps to the target bucket via
  the computed `DeployPhase.bucketType` property (`STAGING` → `BucketType.STAGING`,
  `PRODUCTION` → `BucketType.PRODUCTION`).
- **[Access Key](TN0204_access_key.md)** — the credential used for every bucket operation is
  built from the access key the project deploys through.

A relationship diagram is omitted: `BucketType` is not a persisted data model, and the bucket
names are pure derivations of the project identifier already shown in
[Project](TN0301_project.md).
