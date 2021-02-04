1. Prepare input files for hosting in `/staging`.
   
&nbsp;&nbsp;&nbsp;&nbsp;- Compress files to reduce file size and speed up 
file transfer.
   
&nbsp;&nbsp;&nbsp;&nbsp;- If your jobs need multiple large input files, 
use `tar` and `zip` to combine files so that only a single `tar` or `zip` 
archive is needed per job.

2. Use the transfer server, `transfer.chtc.wisc.edu`, to upload input 
files to your `/staging` directory.

3. Create your HTCondor submit file.

&nbsp;&nbsp;&nbsp;&nbsp;- Include the following submit detail to ensure that
your jobs will have access to your files in `/staging`:

``` {.sub}
requirements = (HasCHTCStaging =?= true)
```

4. Create your executable bash script.

&nbsp;&nbsp;&nbsp;&nbsp;- Use `cp` or `rsync` to copy large input 
from `/staging` that is needed for the job. For example:

```
cp /staging/username/my-large-input.tar.gz ./
tar -xzf my-large-input.tar.gz
```
{:.term}

&nbsp;&nbsp;&nbsp;&nbsp;- If the job will produce output >4GB this output should be 
be compressed  moved to `/staging` before job terminates. If multiple large output 
files are created, use `tar` and `zip` to reduce file counts. For 
example:

```
tar -czf large_output.tar.gz output-file-1 output-file-2 output_dir/
mv large_output.tar.gz /staging/username
```
{:.term}

&nbsp;&nbsp;&nbsp;&nbsp;- Before job completes, delete input copied from `staging`, the 
extracted large input file(s), and the uncompressed or untarred large output files. For example:

```
rm my-large-input.tar.gz
rm my-large-input-file
rm output-file-1 output-file-2
```
{:.term}

5. Remove large input and output files `/staging` after jobs complete using 
`transfer.chtc.wisc.edu`.
