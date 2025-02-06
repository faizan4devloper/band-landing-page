const [emptyKeysPercentage, setEmptyKeysPercentage] = useState(() =>
    location.state?.emptyKeysPercentage ?? localStorage.getItem("emptyKeysPercentage") ?? 100
  );

  useEffect(() => {
    if (emptyKeysPercentage !== null) {
      localStorage.setItem("emptyKeysPercentage", emptyKeysPercentage);
    }
  }, [emptyKeysPercentage]);
