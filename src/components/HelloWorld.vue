<script>
import { ref } from "vue";

const useHelloWorld = () => {
  const value = ref("HelloWorld");

  const setValue = (newValue) => {
    value.value = newValue;
  };

  return { value, setValue };
};

export default useHelloWorld;
</script>
