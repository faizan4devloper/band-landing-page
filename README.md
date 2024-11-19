const { getRootProps, getInputProps } = useDropzone({
    onDrop: (acceptedFiles) => {
      // Handle file selection here
      onFileChange(acceptedFiles);
    },
    multiple: false, // Allow only one file at a time
  });
