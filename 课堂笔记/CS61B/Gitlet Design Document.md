#CS61B
## Classes and DataStructures
### Main
The entry point to our program. It takes and validates the arguments, and calls the corresponding command in `Repository` which will execute the main logic of Gitlet.
#### Fields
No fields in class Main.

### Repository
It is where the major logic lies. It will handle all the commands and set up persistence.
#### Fields
1. `public static final File CWD = new File(System.getProperty("user.dir"))` The Current Working Directory which other methods may use.
2. `public static final File GITLET_DIR = Utils.join(CWD, ".gitlet")` The hidden `.gitlet` directory. This is where all of the state of the `Repository` will be stored.

### Commit
It manages a single snapshot of our working directory, as well as stores the commit's metadata, which includes a commit message and the timestamp. It processes all the operations about a single commit, like creating and checking out commits.
#### Fields
1. `public static File COMMITS_DIR = join(Repository.GITLET_DIR, "commits")` The directory storing all of the commits.
2. `public String message` The commit message of the commit.
3. `public long timestamp` The milliseconds from 1970/1/1 00:00:00 GMT. It can be formatted as a readable string representing the commit time.
4. `private String parent` The sha of the commit's parent commit.
5. `private TreeMap<String, String>` A map that maintains the relationship of the filenames and the file versions (i.e. blobs).

### Blob
~~It's a static class that provides helper functions maintaining blobs. A blob is some version of some specific file.~~
It manages a blob in the repository. A blob is an object that maintains a certain version of some specific file.
#### Fields
1. `public static File BLOBS_DIR = join(Repository.GITLET_DIR, "blobs")` The directory storing all of the blobs.
2. `public String filename` The name of the file that the blob maintains.
3. `public byte[] contents` The contents of the file that the blob maintains.

### Stage
~~A static class that provides helper functions managing the staging area. ~~
It manages the staging area.
#### Fields
1. `public static File STAGING_AREA_DIR = join(Repository.GITLET_DIR, "staging_area")` The directory storing the whole staging area.
2. `public static File STAGED_DIR = join(STAGING_AREA_DIR, "staged")` The directory storing the staged files.
3. `public static File REMOVED_DIR = join(STAGING_AREA_DIR, "removed")` The directory storing the removed files.
### Branch
It manages a Gitlet branch. It deals with the main logic of branching, like branch, rm-branch, merge, etc.
#### Fields
1. `public static File BRANCHES_DIR = join(Repository.GITLET_DIR, "branches")` The directory storing all of the branches.
2. `private String commit` The name of the commit that the branch is on.

## Algorithms

## Persistence
The directory structure looks like this: 
```
CWD                <==== Whatever the current working directory is.
└── .gitlet        <==== All persistant data is stored within here.
    ├── blobs      <==== All blobs are stored here.
    ├── branches   <==== All branches are stored here.
    ├── commits    <==== All commits are stored here. 
    ├── HEAD       <==== The name of the current branch is stored in this single file.
    └── stage <==== Files that have been staged or removed are here.

```
We set up persistence for blobs, branches and commits by implementing `store` and `fromFile` methods for each of them.

For 