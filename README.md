if (location.state?.summary) {
          setSummary({
            briefSummary: location.state.summary, // Use passed summary
          });
          setLoading(false);
          return;
        }
